# MySQL - ROWNUM, ROW_NUMBER()

- MySQL 5.7 이전 버전은 Window함수를 사용할 수 없음. 따라서 오라클에서 쓰는 ROWNUM 내부컬럼이나 ROW_NUMBER()과 같은 분석함수를 사용할 수 없음. (MySQL 8 이후 버전에선 Window함수가 사용이 가능)
- 관련하여 대안workaround가 있는데, 스택오버플로우[^1]를 참조하면 됨
[^1]:https://stackoverflow.com/questions/1895110/row-number-in-mysql#answer-1895127

### 변수와 함께 놀기

Oracle DBMS에 익숙해있으면 `ROWNUM`이라는 PSEUDO 컬럼에 익숙할것이다. `SELECT`로 row를 패치할 때 마다 번호를 붙이는 기능으로, 다양하게 활용이 되는데, MySQL에는 그런 컬럼이 없다.

MySQL의 경우 workaround로 다른 방안을 쓰는데, `@변수명`과 같은 변수를 사용하고, **이를 이용해 프로그래밍을 하는 것이다.**

이 방법을 적용하기 위해선 ~~불행히도~~ **쿼리가 수행되는 순서도 어느정도는 알아야 한다.** 

-   변수는 `SET` 구문에서 `=` 부호를 통해 할당될 수 있다. 

    ```sql
    set @i = 1;  -- 변수 할당

    select @i;  -- 조회
    ```

    |@i
    |--
    | 1

[^2]:https://dev.mysql.com/doc/refman/8.0/en/user-variables.html  

-   `SELECT`등에서도 바로 셋업이 가능하다. 이때는 `:=` 연산자를 통해 변수에 값을 할당할 수 있다. MySQL 8.0 매뉴얼[^2]을 보면 `:=`를 통해 할당을 추천하며, `SET`의 경우 하위버전 호환성을 위해 남겨지게 된다고 한다.

  
    ```sql
    select @j := 1 as j;
    ```

    |j
    |-
    |1

-   한가지 다른 방법이 또 있다. `SELECT` 이후에 `INTO`를 쓰면 됨

    ```sql
    select 1, 2 
      into @i, @j
    ;

    select @i, @j;
    ```

    @i|@j|
    --|--|
    1| 2|

-   다만 `SELECT ~ INTO`는 여러 행을 입력할 경우 에러가 발생한다.

    ```sql
    select a, b
      into @i, @j
      from (select 1 as a, 2 as b
            union all 
            select 3, 4 
        ) x
    ;

    --> SQL Error [1172] [42000]: Result consisted of more than one row
    ```

-   프로그래밍 요소도 있다. 위에서 수행한 `@j`에 `1`만큼을 추가하려면 다음과 같이 하면 된다.

    ```sql
    select @j := @j + 1 as j;
    ```

    |j
    |-
    |2

-   자, 이제 다음의 쿼리를 보면 `SELECT` 에선 뒷쪽 컬럼으로 갈 수록 나중에 수행됨을 알 수 있다.

    ```sql
    set @k = 0;

    select @k := @k + 1
          ,@k := @k + 1
          ,@k := @k + 1;
    ```

    |k1|k2|k3
    |--|--|--
    | 1| 2| 3

-   다음 쿼리를 보면 `FROM`은 `SELECT`보다 먼저 수행됨을 알 수 있다.

    ```sql
    set @k = 0;

    select @k := @k + 1 as k1
          ,@k := @k + 1 as k2
          ,@k := @k + 1 as k3
    from (select @k := 5) x;
    ```

-   다음 쿼리를 통해서는 `FROM`절에서 역시 뒷쪽 테이블이 나중에 수행됨을 예상할 수 있다.

    ```sql
    set @k = 0;

    select @k
    from (select @k := 1) x
        ,(select @k := 2) y
        ,(select @k := 3) z
    ;
    ```

    |@k
    |--
    | 3

-   `CASE`문도 테스트해 볼 수 있다.

    ```sql
    select case when 1=1 then @k := @k + 1
                when 1=1 then @k := @k + 1
           end as a
          ,@k
    from (select @k := 0) x
    ;
    ```

    |a|@k
    |-|--
    |1| 1

-   `CASE`문에서 조건에 맞지 않는 부분은 수행이 되지 않는다는 것.

    ```sql
    select case when 1=1 then @k := @k + 1
                when 1=2 then @k := @k + 1
           end as a
          ,@k
    from (select @k := 0) x
    ;
    ```

    |a|@k
    |-|--
    |1| 1

-   MySQL에서 `0`이 아닌 숫자는 `True`에 해당하기 때문에 다음 `CASE`문에서는 `B`부분만 수행이 된다.

    ```sql
    select case when not(@k := @k + 1) then 'A'
                when (@k := @k + 1) then 'B'
                when not(@k := @k + 1) then 'C'
                else 'D'
           end as a
           ,@k
      from (select @k := 0) x
    ;
    ```

    |a|@k
    |-|--
    |B| 2

    
-   아래 쿼리는 어떻게 해석해야 할까?

    ```sql
    set @k = 0;

    select @k := @k + 1 as k
      from (select @k := 0) x
     where @k := @k + 1
    ;

    ```
    |k
    |--
    | 3

    -   예상에는 `2`가 나올 것 같았다. 기본값 `0`에서 시작해서, `WHERE`에서 `+1`, `SELECT`에서 `+1` 으로 예상했기 때문이다.

-   다음 예제를 보면 동작을 예상하기 더 쉽다.

    ```sql
    select @k := @k + 1 as k1
          ,@k := @k + 1 as k2
          ,@k := @k + 1 as k3
      from (select @k := 0) x
     where @k := @k + 1
    ;
    ```

    |k1|k2|k3
    |--|--|--
    | 3| 4| 5    

    - 뭔가 `WHERE`절에서 2번 카운트 된 것 같다. 





    ```sql
    select staff_no
        , m_id
        , @rownum := @rownum + 1 as rank
    from wmp_right.member_staff_mapping
        ,(select @rownum := 0) x
    where regist_dt >= current_date 
    and staff_no = :staff_no 
    ;
    ```

### ROWNUM


### ROW_NUMBER()

- 각 PARTITION별 `ROW_NUMBER()`등을 하려면 다음과 같이

    ```sql
    select staff_no, m_id
    from (select staff_no
                ,regist_dt
                ,m_id
                ,case when @part = staff_no then @rownum := @rownum + 1 else @rownum := 1 end as rk
                ,@part := staff_no part
            from wmp_right.member_staff_mapping a
                ,(SELECT @rownum := 0, @part := '') r
            order by 1, regist_dt desc
        ) x
    where rk = 1       
    ;
    ```

### 참고


- [MySQL 8.0 Reference Manual / Language Structure / User-Defined Variables
9.4 User-Defined Variables](https://dev.mysql.com/doc/refman/8.0/en/user-variables.html)
- [Advanced MySQL User Variable Techniques](https://www.xaprb.com/blog/2006/12/15/advanced-mysql-user-variable-techniques) : User-defined 변수를 무슨 프로그래밍 하듯이 해서 `ROW_NUMBER`을 구현함


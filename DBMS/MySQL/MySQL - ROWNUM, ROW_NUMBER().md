# MySQL - ROWNUM, ROW_NUMBER()

- MySQL 5.7 이전 버전은 Window함수를 사용할 수 없음. 따라서 오라클에서 쓰는 ROWNUM 내부컬럼이나 ROW_NUMBER()과 같은 분석함수를 사용할 수 없음. (MySQL 8 이후 버전에선 Window함수가 사용이 가능)
- 관련하여 대안workaround가 있는데, 스택오버플로우[^1]를 참조하면 됨
[^1]:https://stackoverflow.com/questions/1895110/row-number-in-mysql#answer-1895127

### ROWNUM

- `ROWNUM`은 다음과 같은 형태로 사용

    ```sql
    select staff_no
        , m_id
        , @rownum := @rownum + 1 as rank
    from wmp_right.member_staff_mapping
        ,(select @rownum := 0) x
    where regist_dt >= current_date /*추출일 당시 해당 조건으로 m_id가 유일함 확인*/
    and staff_no = :staff_no 
    ;
    ```

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


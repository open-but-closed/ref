
### 세션(Session) 관련

- 세션 수 확인

    ```sql
    select a.user_name, count(*)
    from sessions a
    group by 1
    order by 2 desc
    ;
    ```

- 전체 동시접속 가능 세션수 확인

    ```sql
    SHOW DATABASE DEFAULT MaxClientSessions;
    ```

- 동시 접속가능 세션수 확장시 사용

    ```sql
    ALTER DATABASE wmp SET MaxClientSessions = 200;
    ```
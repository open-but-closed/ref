# MySQL Hints

## 참조링크
- [Controlling the Query Optimizer](https://dev.mysql.com/doc/refman/8.0/en/controlling-optimizer.html)

## Optimizer Hints

- SQL 힌트의 경우 다음과 같이 `/*! ... */` 로 하는 것이 쿼리도 MySQL Dependent하게 작성하지 않고, 가독성도 살릴 수 있고,... 여튼 권장

    ```sql
    SELECT /*! straight_join */ * ...
    ```

- `STRAIGHT_JOIN`
    테이블을 열거한 순서대로 처리
    오라클의 `ORDERED`와 유사함


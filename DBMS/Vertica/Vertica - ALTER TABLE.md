# Vertica - ALTER TABLE

```sql
ALTER TABLE [[database.]schema.]table {
    ADD COLUMN [ IF NOT EXISTS ] column datatype
        [ column‑constraint ]
        [ ENCODING encoding‑type ]
        [ PROJECTIONS (projections-list) | ALL PROJECTIONS ]
    | ADD table‑constraint
    | ALTER COLUMN { SET | DROP } expression
    | ALTER CONSTRAINT constraint‑name { ENABLED | DISABLED }
    | DROP CONSTRAINT constraint‑name [ CASCADE | RESTRICT ]
    | DROP [ COLUMN ] [ IF EXISTS ] column [ CASCADE | RESTRICT ]
    | FORCE OUTER integer
    | { INCLUDE | EXCLUDE | MATERIALIZE } [ SCHEMA ] PRIVILEGES
    | OWNER TO owner 
    | partition‑clause [ REORGANIZE ]
    | REMOVE PARTITIONING
    | RENAME [ COLUMN ] name TO new‑name
    | REORGANIZE
    | SET ActivePartitionCount expression
    | SET SCHEMA schema
    | SET STORAGE load‑option
}
```

---

## 이름변경

### Rename Table

- 하나의 테이블에 대한 rename은 다음과 같이 수행하며, rename 대상이 되는 테이블의 schema명은 작성하지 않음. 만약 테이블의 schema를 변경하기 원한다면 `OWNER TO`로 schema를 먼저 변경해야 한다.
   
    ```sql
    ALTER TABLE [[database.]schema.]table[,…] 
    RENAME TO new-table-name[,…]
    ```

    예시

    ```sql
    ALTER TABLE S1.T1 RENAME TO U1;
    ```

    여러 개의 테이블의 이름을 바꿀 수도 있고, 임시 테이블을 이용한 swap도 가능하다. 그리고 아마 하나의 트랜잭션으로 묶여서 수행되는 것으로 알고 있음. (즉, 일부가 안될 경우 전체가 취소 되는 것으로 알고 있음)

    ```sql
    ALTER TABLE S1.T1, S1.T2, temps
    RENAME TO temps, T1, T2;
    ```

---
## 오너

### Change Owner

- 테이블 소유자(schema)를 변경하려면 다음과 같이 수행

    ```sql
    ALTER TABLE t1 OWNER TO Alice;
    ```
---

## 컬럼

### Add Column

- 그냥 하면 됨. 여러 컬럼을 한번에는 안되고, 여러 컬럼일 경우 반복해서 수행.

    ```sql
    ALTER TABLE [[database.]schema.]table 
    ADD COLUMN [ IF NOT EXISTS ] column datatype
            [ column‑constraint ]
            [ ENCODING encoding‑type ]
            [ PROJECTIONS (projections-list) | ALL PROJECTIONS ]
    ;
    ```
    
### Rename Column

- 다음과 같음

    ```sql
    ALTER TABLE [schema.]table-name  
    RENAME [ COLUMN ] column-name TO new-column-name
    ```

### Modify Column

- 주로 데이터 타입이나 제약조건을 변경할 때 사용할 듯

    ```sql
    ALTER TABLE exploded_names 
    ALTER COLUMN time_stamp 
    SET DATA TYPE TIMESTAMPTZ;
    ```

## Drop Column

- 그냥 하면 됨;

    ```sql
    ALTER TABLE [[database.]schema.]table
    DROP [COLUMN] [IF EXISTS] column [CASCADE | RESTRICT]
    ```


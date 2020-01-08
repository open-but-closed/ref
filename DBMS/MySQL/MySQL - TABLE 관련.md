# MySQL - TABLE 관련 

- MySQL 테이블 생성 관련 자주 쓰는 옵션
- 원 링크는 이곳[^1]을 참조
[^1]:https://dev.mysql.com/doc/refman/8.0/en/create-table.html

## MySQL - CREATE TABLE

### CREATE 정의

- 테이블 생성 옵션
    - `IF NOT EXISTS` 키워드로 에러 방지 가능


- 인덱스 생성
    - `index`키워드나 `key`키워드를 이용하면 됨

    ```sql
    CREATE TABLE tbl_name
    (
        col1 varchar(10)
        ,INDEX (col1, col2, ...)
        ,KEY (col1, col2, ...)
        ,INDEX 인덱스명 (col1, col2, ...)
        ,KEY 인덱스명 (col1, col2, ...)
    );
    ```

- PK 생성

    ```sql
    CREATE TABLE tbl_name
    (
        col1 varchar(10)
        ,PRIMARY KEY (col1, col2, ...)
        ,CONSTRAINT PRIMARY KEY (col1, col2, ...)
        ,CONSTRAINT PK명 PRIMARY KEY (col1, col2, ...)
    );
    ```

- Unique 인덱스 생성

    ```sql
    CREATE TABLE tbl_name
    (
        col1 varchar(10)
        ,UNIQUE KEY (col1, col2, ...)
        ,CONSTRAINT UNIQUE KEY (col1, col2, ...)
        ,CONSTRAINT PK명 UNIQUE KEY (col1, col2, ...)
    );
    ```

### 컬럼

- 컬럼






```








create_definition:
    col_name column_definition
  | {INDEX|KEY} [index_name] [index_type] (key_part,...)
      [index_option] ...
  | {FULLTEXT|SPATIAL} [INDEX|KEY] [index_name] (key_part,...)
      [index_option] ...
  | [CONSTRAINT [symbol]] PRIMARY KEY
      [index_type] (key_part,...)
      [index_option] ...
  | [CONSTRAINT [symbol]] UNIQUE [INDEX|KEY]
      [index_name] [index_type] (key_part,...)
      [index_option] ...
  | [CONSTRAINT [symbol]] FOREIGN KEY
      [index_name] (col_name,...)
      reference_definition
  | check_constraint_definition

column_definition:
    data_type [NOT NULL | NULL] [DEFAULT {literal | (expr)} ]
      [AUTO_INCREMENT] [UNIQUE [KEY]] [[PRIMARY] KEY]
      [COMMENT 'string']
      [COLLATE collation_name]
      [COLUMN_FORMAT {FIXED|DYNAMIC|DEFAULT}]
      [STORAGE {DISK|MEMORY}]
      [reference_definition]
      [check_constraint_definition]
  | data_type
      [COLLATE collation_name]
      [GENERATED ALWAYS] AS (expr)
      [VIRTUAL | STORED] [NOT NULL | NULL]
      [UNIQUE [KEY]] [[PRIMARY] KEY]
      [COMMENT 'string']
      [reference_definition]
      [check_constraint_definition]

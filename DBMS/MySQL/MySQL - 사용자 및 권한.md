---
title: MySQL - 사용자 및 권한
---


- 음

    ```sql
    select *
    from information_schema.USER_PRIVILEGES
    where 1=1
    and grantee like '%192.168%' 
    and grantee like '%stat%' 
    order by 1   
    ```

    ```sql
    select *
    from information_schema.SCHEMA_PRIVILEGES
    where 1=1
    and grantee like '%stat_etl%'
    --    and table_schema like 'wmp_dwstat'
    order by 1,2,3,4
    ``` 
    

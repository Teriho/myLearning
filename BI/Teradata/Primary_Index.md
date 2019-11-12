### PI - Primary Index

##### It is used for:

- Data distribution
- known access path
- improve join performance
- provides fastest path to access data and avoid full table scan

##### Types of PI:

- Unique PI (UPI)
- Non-Unique PI (NUPI)

##### Constrains

- PI must be created when creating the table
- only one for one table

##### Steps when inserting values with UPI:

1. ROW id will be a 32-bit hash value + a uniqueness value
2. According to the Hash Map, the PE will send the row to AMP

##### Steps when inserting values with NUPI:

1. The same process of generating Row-Hash value
2. The unique value will automatically increased, and added this to hash-value.
3. all the duplicated values will go the same AMP, which leads to uneven distribution of data and may cause bad performance

### PPI - Partitioned Primary Index

> PPI allows user to access part of the table

##### 4 Types

- Case partitoning
- Range based partitioning
- Multi-level partitioning
- character based partitioning

##### Examples

![figure_2](/Users/ericyang/GoogleDrive/GitHub/myLearning/BI/Teradata/src/figure_2.png)

1. Range Partitioning :

```sql
CREATE TABLE PRODUCT_TABLE ( 
    ORDER_NO INTEGER NOT NULL, 
    CUST_NO INTERGER,
    PRODUCT VARCHAR(20), 
    ORDER_DATE DATE, 
    AREA VARCHAR(50), 
    ORDER_COSTDECIMAL(10,2) ) 
PRIMARY INDEX(ORDER_NO) 
PARTITION BY RANGE_N (ORDER_DATE BETWEEN DATE '2015-01-01' 
    AND DATE '2015-12-31' EACH INTERVAL '7' DAY);
```

2. Case Partitioning :

```sql
CREATE TABLE PRODUCT_TABLE
(
    ORDER_NO INTEGER NOT NULL,
    CUST_NO INTERGER,
    PRODUCT VARCHAR(20),
    ORDER_DATE DATE,
    AREA VARCHAR(50),
    ORDER_COSTDECIMAL(10,2)
)
PRIMARY INDEX(ORDER_NO)
PARTITION BY CASE_N(ORDER_COST< 1000,
     ORDER_ COST< 2000,
     ORDER_ COST< 5000,
     NO CASE, UNKNOWN);
```

3. Multilevel Partitioning :

```sql
CREATE TABLE PRODUCT_TABLE
(
    ORDER_NO INTEGER NOT NULL,
    CUST_NO INTERGER,
    PRODUCT VARCHAR(20),
    ORDER_DATE DATE,
    AREA VARCHAR(50),
    ORDER_COSTDECIMAL(10,2)
)
PRIMARY INDEX(ORDER_NO)
PARTITION BY (RANGE_N(ORDER_DATE BETWEEN DATE '2015-01-01' AND DATE '2015-12-31'
    EACH INTERVAL '1' DAY)
    CASE_N (ORDER_TOTAL < 1000,
         ORDER_TOTAL < 2000,
         ORDER_TOTAL < 3000,
         NO CASE, UNKNOWN));
```

4. Character based Partitioning :

```sql
CREATE TABLE EMPLOYEE (
       EMP_ID INTEGER,
       LAST_NAME VARCHAR (30) CHARACTER,
       FIRST_NAME VARCHAR(30),
       CITY VARCHAR(50))
     PRIMARY INDEX (EMP_ID)
     PARTITION BY 
        CASE_N (LAST_NAME LIKE 'A%', 
                LAST_NAME LIKE 'B%',
                LAST_NAME LIKE 'C%', 
                LAST_NAME LIKE 'D%',
                LAST_NAME LIKE 'E%', 
                LAST_NAME LIKE 'F%',
                NO CASE, UNKNOWN);
```

##### Pros

- Avoid full table scan
- Fast insert, delete operations
- Can be created on global temp table, volatile table, non-compressed join indexes
- For range based queries we can remove SIs and use PPI, which will reduce space of an overhead SI subtable 

##### Cons

- An additional of 2 bytes is added to each row and hence increases the perm space.
- While joining a PPI table with non-partitioned table may take a long time.
- Access other than the PPI column may take more time.
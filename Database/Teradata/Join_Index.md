A Join Index takes two or more tables and **physically joins** the tables together into **another physical index table**. It also **updates** the Join Index table when the base rows of the joining base tables are updated.

### STJI - Single Table Join Index

> STJI is used to create indexing structure for a single table but with different primary index.

```sql
CREATE JOIN INDEX <index name> 
AS 
<SELECT Query> 
<Index Definition>;
```

The PE will use PI to query the base table, if no PI on the query column, the PE will then use JI to query. So STJI is more likely a PI.

### MTJI - Multi Table Join Index

> MTJI is used to hold pre-join result.

```sql
CREATE JOIN INDEX Employee_Salary_JI 
AS 
SELECT a.DepartmentNo,SUM(b.NetPay) AS TotalPay 
FROM Employee a 
INNER JOIN Salary b 
ON(a.EmployeeNo = b.EmployeeNo)
GROUP BY a.DepartmentNo 
Primary Index(DepartmentNo); 
```



### Aggregate Join Index

> It can be used to improve performance on aggregation purpose. 

```sql
CREATE JOIN INDEX SALES_INX STORE_NO, SUM(QTY_SOLD) 
FROM SALES_BASE_TABLE 
GROUP BY 1 
ORDER BY 2;
```

It can only be used for:

- SUM
- COUNT
- GOURP BY

### Sparse Join Index

> When the base table is large, this feature can be used to reduce the content of the join index to only a portion of the table.

```sql
CREATE JOIN INDEX SPARSE_BILL_INX
AS
SEL ACC_NO, CUST_NAME, BILL_DATE, BILL_AMT
FROM BILL_DETAILS
WHERE BILL_YEAR=2015
UNIQUE PRIMARY INDEX(ACC_NO);
```



### Limitation of Join Index

- The Join indexes are not supported by Fastload and Multiload utilities, They must be dropped and recreated after the table is loaded.
- During restore of a base table or database join index is marked as an invalid.
- Max 64 columns per table per Join Index.
- Join Index Subtables cannot be fall back protected.
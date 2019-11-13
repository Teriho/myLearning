Hash index in Teradata has been designed to improve the performance of Teradata query in the way similar to the Single Table Join Index, which restricts parsing engine not to access or redistribute the base table. It may act like secondary index by providing an alternative access path for the base table.

> Hash Index is a restricted form of STJI, so their performance are the same 

### Syntax

```sql
CREATE HASH INDEX EMP_HI (
    EMP_SSN_NO,
    DEPT 
)
ON EMPLOYEE;
```

### Limitation

- must have PI
- cannot define HI on PPI


### SI - Secondary Index

> SI can be created/droped after the creation of the table

##### Types of SI

- Unique Secondary Index (USI)
- Non-unique secondary Index (NUSI)

##### Subtable

once defined the SI, subtable will be created automatically on every AMP, and subtable contains:

- The secondary index value
- The secondary index row_id
- The base table row_id

##### Steps of USI Creation

1. Create SI

```sql
CREATE UNIQUE INDEX (column_name) on dbname.tablename;
```

2. Create subtable on each AMP( Automatic)
3. USI column values will send to PE for hashing

##### Steps of USI Access

1. Hashing algorithm hashes the USI value
2. Using hash map, it will find the AMP number where SI value stored
3. go to the subtable and fetch the row-id of the base table
4. Using hash map again to fetch the amp number where the base table resides

##### difference between USI and NUSI

- USI subtable rows are hashed
- NUSI subtable rows are AMP-local
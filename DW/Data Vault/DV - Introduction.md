### Resources

- [Agile Modeling: Not an Option Anymore](https://www.vertabelo.com/blog/data-vault-series-agile-modeling-not-an-option-anymore/)

- [Data Vault 2.0 Modeling Basics](https://www.vertabelo.com/blog/data-vault-series-data-vault-2-0-modeling-basics/)
- [The Business Data Vault](https://www.vertabelo.com/blog/data-vault-series-the-business-data-vault/)

### Purpose

The Data Vault System of Business Intelligence provides (among other things) a method and approach to modeling your enterprise data warehouse (EDW) that is agile, flexible, and scalable.

### Definition

The Data Vault is a detail oriented, historical tracking and uniquely linked set of normalized tables that support one or more functional areas of business.

DV 2.0 is not just about the modeling. It also includes best practices for architecture, loading and methodology. The approach has been expanded to specifically include an agile method based on Scott Ambler’s Disciplined Agile Development.

### Key Features

- Flexibility
- Productivity
- Scalability
- Auditability
  - DV requires that we load data exactly as it exists in the source

### Use Case

- In Bill Inmon’s Corporate Information Factory (CIF) approach, the DV fits right between the Stage layer and the Data Mart Layer

- In a Kimball Bus Architecture, the DV becomes the Persistent Staging Area that feeds the Dimensional Data Warehouse.

### Component

> In short, 
>
> - Hubs = Business Keys
> - Links = Associations / Transactions
> - Satellites = Descriptors

#### Hub

A Hub is a list of unique business key. It contains:

> Business key: meaningful, like name. not to use surrogate key.

- A Hub PK
  - is a calculated field consisting of a Hash of the business key
- Businesis Key column( BK)
  - unique
  - can be compound key made up of more than 1 column
- Load Date ( LOAD_DTS)
  - the first time of the BK 
- Source for the record( REC_SRC)
  - data source

![image-20191106142544118](/Users/ericyang/GoogleDrive/L&D/L&D_Repo/myLearning (1)/DW/Modelling/Data Vault/image-20191106142544118.png)

> Location_Name is the BK, and it must be unique

- Hash-based PK

  - Hashing on PK will allow DV2.0 to be deployed on Hadoop. (Hadoop don't have a surrogate key generator.)

  - Sample code will be like:

  - ```sql
    dbms_obfuscation_toolkit.md5(upper(trim(location_name)))
    ```

#### Links

Link is to capture and record the relationship of data elements. It contains:

- Link PK
- PKs from the parent Hubs
- BK columns
- Load Date (LOAD_DTS)
- Source for the record (REC_SRC)

![image-20191106143439709](/Users/ericyang/Library/Application Support/typora-user-images/image-20191106143439709.png)



#### Satellites

These structures are where all the descriptive columns go, and where the the Change Data Capture( CDC) is done and history is stored. It contains:

- Hub PK ( AS Satellites PK and FK)

- HUB LOAD_DTS ( AS Satellites PK)

- Hub descriptive columns

- Source for the record (REC_SRC)

- HASH_DIFF

  - Hash all the descriptive columns  for determining if a record in the source has changed.

  - Examples: 

  - ```sql
    SAT_LOCATIONS.HASH_DIFF =
       dbms_obfuscation_toolkit.md5(
          TRIM(street_address) || ’^’ || TRIM(city) || ^ || TRIM(state_province) 
          || ’^’ || TRIM(postal_code))
    ```

![image-20191106145747941](/Users/ericyang/Library/Application Support/typora-user-images/image-20191106145747941.png)

#### PIT Tables

A Point-in-Time (PIT) table is a modified Satellite that helps when we need to query data from a Hub that has multiple Satellites.

![image-20191106152058911](/Users/ericyang/Library/Application Support/typora-user-images/image-20191106152058911.png)

- A Hub may have serveral Sats because:
  - Multiple source
  - the rate of change of certain attributes if faster/slower than others
  - specific data classification
- It will be very complex when query. So PIT table will reduce the join when quey.
- When adding new Sats to your DV, be sure to check for PIT tables that need updating as well

#### Bridge Tables

When we need to query across serveral tables, we might have to join them. However, with bridge tables, which contains all the keys needed to get all the data from the right Sats.

> - Also add a PIT table on the bridge table in order to truly complete this structure
>
> - Link tables now can be used as bridge tables

![image-20191106153430892](/Users/ericyang/Library/Application Support/typora-user-images/image-20191106153430892.png)

#### Predefined Derivations

Predefined Derivations tables should be structured to look like Links or Sats attached to existing Hubs or Links. In short, these tables contains the aggregations or the most common used fields in order to increase the flexibility.

- No Hash_DIFF  or REC_SRC required, as thses tables are populated on demand.
### Overview

It is a end-to-end data integration and analytics platorm, quickly transforming all your raw data into easily accessible, analytics-ready information and deliver game-changing insights with a noe-of-a-kind, mordern data and analytics platform



### Competitors

- Tableau
- Tibco Spotfire
- Visokio Omniscope

### Getting Started

#### 1. Resources

- [This tutorial](https://help.qlik.com/en-US/qlikview/12.1/Content/Tutorial.htm) provides an overview of the main features and capabilities availiable of the QlikView
- [Qlik Sense free trail](https://www.qlik.com/us/try-or-buy?_ga=1.215122711.1281566144.1474266387)

#### 2.Installation

[Getting Started with QlikView - Download and Install](https://community.qlik.com/t5/QlikView-Documents/Getting-Started-with-QlikView-Download-and-Install/ta-p/1492558?_ga=1.181101607.1281566144.1474266387)

#### 3. QlikView and Hadoop

##### Research

##### http://qlik.com
- Slowest website ever?
- Search 'hortonworks' - 1 result: Data Sources
	- Hortonworks data source driver/connector: Native + [Hortonworks](http://hortonworks.com/hdp/) ODBC driver

##### [https://community.qlik.com](https://community.qlik.com/)

- No official results
- How to Connect Hadoop into Qlikview?
	- May 2016
	- MapR Hive ODBC
- [How to connect QlikView with Hadoop?](https://community.qlik.com/thread/114744) Apr-2014 question, answers to 2011 and 2013 posts
- [http://hortonworks.com/hadoop-tutorial/business-discovery-and-visualizing-your-data-in-hdp-using-qlikview/](http://hortonworks.com/hadoop-tutorial/business-discovery-and-visualizing-your-data-in-hdp-using-qlikview/)

##### Install HDP ODBC driver

- Download [Hortonworks ODBC Driver for Apache Hive (v2.1.5) for Windows 64-bit](http://public-repo-1.hortonworks.com/HDP/hive-odbc/2.1.5.1006/windows/HortonworksHiveODBC64.msi)
- Install on Windows VM
- Configure ODBC - to connect to remote HDP :
	- See [Open the ODBC Data Source Administrator](https://msdn.microsoft.com/en-us/library/ms188691.aspx) 
	- Change host(s) to the name/IP-address of your HDP
	- Click 'Test' - should give SUCCESS

##### [QlikView database connection](https://www.tutorialspoint.com/qlikview/qlikview_database.htm)

##### Use Hive to present data landed in sandbox as SQL-like table

```sql
ODBC CONNECT TO [Sample Hortonworks Hive DSN] (XUserId is TddKdWFMSLaCWPRMFTdOK, XPassword is acMJfWFMSLaCWPRMFTdSO);

webloganalytics:
SQL SELECT * FROM HIVE.personal_maria_dev.webloganalytics;

store webloganalytics into C:\Users\Public\HDP_Sandbox\webloganalytics.qvd;
```



### Certification

- **[Data Literacy & Analytics Certification](https://qcc.qlik.com/mod/url/view.php?id=20097&_ga=2.147202935.1524714829.1572599744-1426038766.1572599744)**
- **[Qlik Sense & QlikView Certifications( expert)](https://home.pearsonvue.com/qlik)**
  - Qlik Sense Business Analyst Certification
  - Qlik Sense Data Architect Certification
  - Qlik Sense System Administrator Certification
- **Qlik Sense Qualifications ( fundamental)**



### Licensing/ Pricing

[Pricelist](https://www.qlik.com/us/pricing)



### PoC










## Why Qlik Tech have scalability problems

- QlikTech is “in-memory-only” architecture i.e. they bring all the data that they want to analyze into a proprietary cube. As a result, scalability is limited to the amount of free RAM available, and there will always be much less available RAM than available disk space. In addition, users are confined to perform analysis within the cube boundaries. As a result, the report designer will have to create reports/cubes keeping in mind every possible reporting scenario that end users may want to analyze. This puts additional pressure on hardware resources.
-  RAM need by QlikTech is not only affected by the amount of data, but also by the number of users simultaneously accessing the QlikView application. Each QlikView user needs to have its own User Session States; storing the User Session States and aggregates takes up RAM above and beyond the RAM used to store the QlikView application. It is not uncommon to have a QlikView application that take up nearly 100% of the RAM for each additional user. Having 3-5 concurrent users accessing the same QlikView application could easily double the amount of RAM required, creating serious scalability, performance, and user concurrency issues.
- Each year DB vendors spend millions of dollars in R&D to improve their performance, MicroStrategy  benefits from these improvements transparently as we have a push-down architecture and we use  database for most of our processing; QlikTech cannot leverage these investments as the entire database is  held in-memory.
- QlikTech relies on the client or mid-tier server to perform all its processing. The database to be queried is  held entirely in memory. Data joins are pre-processed while loading the QlikView database, but analytic  calculations and aggregations are performed on-the-fly by the client or midtier. As a result, performance  is bounded by the CPU and memory capacity of the mid-tier server or client.

Regardless of what fancy technology is used, storing the entire database into RAM has inherent scalability limitations:

1. The amount of data you can query with in-memory technology is limited by the amount of free RAM available, and there will always be much less available RAM than available disk space.
2. RAM you need is not only affected by the amount of data you have, but also by the number of people simultaneously querying it. Having 5-10 people using the same in-memory BI application could easily double the amount of RAM required.
3. Hardware costs may increase beyond what you are willing to spend as the application and users grow, and successful BI applications grow fast 

Another point is that scalability not only is more data and more users, it also includes report scalability (this is another beast that no one talks about... we should bring this into discussion) 

Report scalability (Object oriented metadata story...): For true enterprise-class report/dashboard scalability, the BI architecture should allow each report to be **developed more quickly** than the last report, provide **easy maintainability**, and should minimize the number of reports that need to be explicitly defined and maintained in the first place.

Report to be **developed more quickly** than the last report -- QlikView does not have an object-oriented metadata. As a result, the metadata objects have to be created redundantly and almost the same amount of efforts will be required to create new applications.

Provide **easy maintainability** -- For QlikView administrators maintenance also becomes a challenge because if there is a change in a common business definition then users will have to change all the applications (that are affected by the change in business definition) manually (so no automatic change propagation)

**Minimize the number of reports** that need to be explicitly defined -- QlikView does not have the robust prompting or Drill-Anywhere capabilities that we have. For example, prompts on filters, attributes, hierarchies, etc are not supported. Only basic element list prompting can be simulated by the way of drop down boxes or other selector types. Drilling has to be pre-defined. QliKView’s limited prompting/drilling capabilities means that more reports will have to be defined per user resulting in more system resources consumption, more development efforts, and more maintenance efforts. In addition, users cannot navigate out of a pre-processed cube. As a result, the report designer will have to create cubes for every possible reporting scenario that end users may want to analyze. This will obviously put pressure on hardware resources and additional report development.

As far as QlikTech goes they have 15,000 customers and very few of their customers have implementation greater than 2 TB (chart 1 below, QlikTech did not even appear in this chart). Median volume of data analyzed by QV customers is 2.2 GB (Chart 2 below)

![Screen Shot 2019-11-03 at 13.30.59](/Users/ericyang/Desktop/Screen Shot 2019-11-03 at 13.30.59.png)

![Screen Shot 2019-11-03 at 13.31.09](/Users/ericyang/Desktop/Screen Shot 2019-11-03 at 13.31.09.png)


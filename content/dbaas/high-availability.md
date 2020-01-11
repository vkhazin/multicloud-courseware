# High Availability

* Involves multiple components working together to ensure uninterrupted service during a specific period
* With 12factor app can scale out and in, across networks, and regions, but where is the data?
* Five Nines availability 99.999% translates to 5.25 minutes downtime per year
* A highly available database on non-highly available infrastructure or electricity/cooling system?
* The critical element of high availability systems is eliminating single points of failure by achieving redundancy on all levels
* Not as easy to build from scratch
* Even harder to achieve a scalable and highly-available system
* DBaaS ensures high availability by placing servers in multiple availability zones, read: physical data centres
* Some DBaaS offer cross-region data availability, e.g. [Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/distribute-data-globally), [Aurora DB, ](https://aws.amazon.com/rds/aurora/global-database/)and [Cloud Spanner](https://cloud.google.com/blog/products/gcp/with-multi-region-support-in-cloud-spanner-have-your-cake-and-eat-it-too)




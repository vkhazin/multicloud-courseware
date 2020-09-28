# Advantages & Liabilities

* How many Sql Server, Oracle, MySql, Cassandra, MongoDB Ops can you afford?
* An alternative is to "standardize" on the database falling into [golden hammer anti-pattern](https://deviq.com/golden-hammer/)
* Common concerns DBaaS addresses through self-service and automation:
  * Rapid Provisioning
  * Backups, recovery, tuning, optimization, patching, and upgrading
  * Enhanced Security: encryption at rest, encryption on wire, no default dba password
  * Change and usage tracking and auditing
  * Scalability: it is never as simple as it sounds, especially, with Rdbms
* The dark-side of "outsourcing"
  * What to do when a cloud provider shows an outage for DBaaS on their status page?
  * How can we upgrade to the latest version or launch a historical version of the database?
  * We have been using Azure Cosmos DB, how can we embrace AWSand GCP?
  * Is there a break-even point for the cost of DBaaS? 


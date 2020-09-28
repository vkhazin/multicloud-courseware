# Inter-cloud Practices

## Self-managed

* Run a self-managed database on VM's or in containers with [K8s persistent volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
* Establish a replication over VPN between cloud different providers
* Take full responsibility for the Db provisioning, administration, and operation
* Data can be made available across multiple cloud providers

## Data Access Adapters

* Make use of reasonably compatible DBaaS products, e.g.: AWSMySql Rds, Azure MySql, GCP Cloud Spanner
* The provider is responsible for provisioning, administration and operation
* Implement adapters to access different databases to resolve the differences while keeping business logic
* Data is available on a single data provider or a custom replication is required
* Maybe, [blockchain distributed databases](https://www.bigchaindb.com/features/) will emerge to close the gap


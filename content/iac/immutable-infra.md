# Immutable Infrastructure

* No in-place upgrades: create a new version of VM, load-balancer, and etc
* Infrastructure is versioned in a source control system
* Switch to a new version instead of upgrading the existing version
* Reduced risk of upgrades with easier rollback to a previous version
* Downside: no internal application state/data will be required




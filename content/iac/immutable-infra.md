# Immutable Infrastructure

* No in-place upgrades: create a new version of VM, load-balancer, and etc
* Infrastructure is versioned and definitions are in a source control system
* Switch to a new version and retire an existing version, or run both
* Reduced risk of upgrades with easier rollback to a previous version
* Highly reproduce-able
* Downside: internal application state/data will be an impediment




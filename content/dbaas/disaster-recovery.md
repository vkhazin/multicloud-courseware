# Disaster Recovery

* Allows an organization to maintain or quickly resume mission-critical functions following a disaster
* High-availability is not a replacement for disaster recovery, e.g.: `delete from very-important-table`
* A multi-tier/multi-component application can be affected at many levels
* In the worst-case scenario, code can be re-deployed, the data, however, needs to be recovered
* Where are your backups and how soon the database can be recovered?
* DBaaS tends to have automated snapshots for a relatively short period of time: days
* Longer-term backups can be stored in cloud storage available in multiple regions
* Consider where to draw the line: do you need a disaster recovery plan to survive [the sun becoming a red star?](https://www.space.com/14732-sun-burns-star-death.html)




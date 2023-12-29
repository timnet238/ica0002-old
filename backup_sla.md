## Backup SLA

### Backup coverage
<!--- what is backed up and what is not -->

Data from MySQL and InfluxDB will be backed up. The Ansible repository will also be backed up.

MySQL and InfluxDB contain user or log information that cannot be restored any other way. 
The rest of the services, including MySQL and InfluxDB configuration, can be restored from
scratch quickly using Ansible.

The Ansible configuration (https://github.com/jp1995/ica0002) is also backed up on a local server.

### Backup RPO
<!--- Recovery Point Objective - How often are backups done -->

Under 24 hour data loss is considered acceptable. Backups are done once every day, at 01:00 am.

### Versioning and retention
<!--- How many backup versions are stored and for how long
version_count = frequency * retention -->

Backups are stored for two weeks, which results in 14 backup versions at any given time. 
There are three copies of every backup, two stored locally, and one remotely.

### Usability checks
<!--- How is backup usability verified -->

Backups are tested by restoring them on docker containers and inspecting the databases. 
Simple inspection can be easily automated with a script that checks when some often used table
was last updated.
More detailed checks can be done manually once every month, or when new tables or databases are added.

### Restoration Criteria
<!--- When should a system be restored from backup -->

A backup should be restored when an incident happens that renders the data stored irrecoverable
and / or the system inoperable. 

Restoring the backup should always be the last option. First the following steps should be taken.

* Restarting problematic services
* Inspecting logs, general troubleshooting
* Applying fixes to configuration

If these steps do not resolve the incident, and it is certain that a restore from backup is
necessary, then it should be done.

### RTO
<!--- Recovery Time Objective - Guarantee that the service will be restored in this time -->

After an incident, the system should be restored within 4 hours.

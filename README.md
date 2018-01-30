# ansible-role-postgresql
Ansible role for installing and configuring PostgreSQL on an Ubuntu server


## Requirements

None, but this role does need to be run as root, which can be done by running this role from a playbook that has become: yes


## Role Variables

The following variables with their default values are listed below.

```

```


In addition to the above variables, there are a variety of variables associated with the postgresql.conf file that can be set. These variables are grouped into sections and further detail can be found in the ./defaults/main.yml file as well as in the official PostgreSQL documentation (links below):

* [File Locations](https://www.postgresql.org/docs/current/static/runtime-config-file-locations.html)
* [Connections & Authentication](https://www.postgresql.org/docs/current/static/runtime-config-connection.html)
* [Resource Consumption](https://www.postgresql.org/docs/current/static/runtime-config-resource.html)
* [Write Ahead Log](https://www.postgresql.org/docs/current/static/runtime-config-wal.html)
* [Replication](https://www.postgresql.org/docs/current/static/runtime-config-replication.html)
* [Query Planning](https://www.postgresql.org/docs/current/static/runtime-config-query.html)
* [Error Reporting & Logging](https://www.postgresql.org/docs/current/static/runtime-config-logging.html)
* [Runtime Statistics](https://www.postgresql.org/docs/current/static/runtime-config-statistics.html)
* [Automatic Vacuuming](https://www.postgresql.org/docs/current/static/runtime-config-autovacuum.html)
* [Client Connection Defaults](https://www.postgresql.org/docs/current/static/runtime-config-client.html)
* [Lock Management](https://www.postgresql.org/docs/current/static/runtime-config-locks.html)
* [Version & Platform Compatibility](https://www.postgresql.org/docs/current/static/runtime-config-compatible.html)
* [Error Handling](https://www.postgresql.org/docs/current/static/runtime-config-error-handling.html)


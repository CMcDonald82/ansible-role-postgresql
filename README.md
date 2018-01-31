# ansible-role-postgresql

[![Build Status](https://travis-ci.org/CMcDonald82/ansible-role-postgresql.svg?branch=master)](https://travis-ci.org/CMcDonald82/ansible-role-postgresql)

Ansible role for installing and configuring PostgreSQL on an Ubuntu server

## Requirements

None, but this role does need to be run as root, which can be done by running this role from a playbook that has become: yes


## Role Variables

The following variables with their default values are listed below.

```
postgresql_version: 10
```

The version of PostgreSQL to install. NOTE: Currently only version 10 is supported but it is possible that some of the 9.x versions could be supported in the future

```
postgresql_locale: 'en_US.UTF-8' 
postgresql_ctype: 'en_US.UTF-8'
postgresql_encoding: 'UTF-8'
```

These specify the locale, ctype, and encoding. Explanations can be found [here](https://www.postgresql.org/docs/current/static/locale.html). May want to use the value 'C' for these since the Postgres docs (linked above) state that any other values may cause negative performance impact

```
postgresql_admin_user: postgres
postgresql_admin_group: "{{ postgresql_admin_user }}"
```

This will be the main admin user, which is only allowed to connect from localhost, mainly for provisioning, maintenance and scripts. It is created automatically by the installation procedure when Postgres is installed.

```
postgresql_cluster_name: main
postgresql_cluster_reset: true
```

Cluster management (used mostly for cluster commands from postgresql-common package (Ubuntu-specific)) added to process titles if nonempty.

```
postgresql_apt_key_id: ACCC4CF8
postgresql_apt_key_url: "https://www.postgresql.org/media/keys/{{ postgresql_apt_key_id }}.asc"
postgresql_apt_repository: "deb http://apt.postgresql.org/pub/repos/apt/ {{ ansible_distribution_release }}-pgdg main"
```
Settings for Postgres apt key (required for installation). See [this link](https://www.postgresql.org/download/linux/ubuntu/)

```
postgresql_dependencies: 
  - python3-psycopg2
```

List of packages that are dependencies for Postgres.
Need python3-psycopg2 since ubuntu 16.04 ships with Python 3.

```
postgresql_server_start_conf: auto
```

This setting is used in the start.conf file to specify the automatic startup configuration of the cluster

```
postgresql_databases: []
```

Databases to ensure exist.
Specify entries as a list of hashes having this schema:
```
  - name: exampledb # required; the rest are optional
    lc_collate: # defaults to 'en_US.UTF-8'
    lc_ctype: # defaults to 'en_US.UTF-8'
    encoding: # defaults to 'UTF-8'
    template: # defaults to 'template0'
    login_host: # defaults to 'localhost'
    login_password: # defaults to not set
    login_user: # defaults to '{{ postgresql_admin_user }}'
    login_unix_socket: # defaults to 1st of postgresql_unix_socket_directories
    port: # defaults to not set
    state: # defaults to 'present'
```



```
postgresql_users: []
```

Users to ensure exist.
Specify entries as a list of hashes having this schema:
```
  - name: exampleuser #required; the rest are optional
    password: # defaults to not set
    priv: # defaults to not set
    role_attr_flags: # defaults to not set
    db: # defaults to not set
    login_host: # defaults to 'localhost'
    login_password: # defaults to not set
    login_user: # defaults to '{{ postgresql_user }}'
    login_unix_socket: # defaults to 1st of postgresql_unix_socket_directories
    port: # defaults to not set
    state: # defaults to 'present'
```



```
postgresql_hba_entries:
  - type: local
    user: "{{ deploy_username }}"
    method: md5
    database: all
```

Authenticated connections that will be inserted in `pg_hba.conf`. These won't overwrite the default local idents, which are left untouched as they are useful and safe.
Specify entries as a list of hashes having this schema:
```
  - type: local
    user: postgres
    method: peer
    database: 'all'             # defaults to 'samerole', can also be a list
    address: '192.168.0.0/24'   # optional
    options:                    # optional
      key: value
```



```
postgresql_ident_entries: []
```

Authenticated connections that will be inserted in `pg_ident.conf`. These won't overwrite the default local idents, which are left untouched as they are useful and safe.
Specify entries as a list of hashes having this schema:
```
  - name: map_a
    user: system_user
    pg_user: postgresql_user
```



```
postgresql_env: {}
```

A dict whose `key: value`s will be provided as environment variables for the postmaster. Use the format:
```
  - KEY1: value1
  - KEY2: value2
```



```
postgresql_pg_ctl_options: []
```

Automatic pg_ctl configuration. Specify a list of options containing cluster specific options to be passed to pg_ctl(1).

---

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


## Example Playbook

```
- name: Setup Ubuntu server with PostgreSQL 10
  hosts: all
  remote_user: "{{ remote_username }}"
  become: yes

  roles:
    - role: postgresql_role
      postgresql_databases:
        - name: exampledb 
          lc_collate: 'en_US.UTF-8'
          lc_ctype: 'en_US.UTF-8'
          encoding: 'UTF-8'
          template: 'template0'
          login_host: 'localhost'
          login_password: 'abc123'
          login_user: "{{ postgresql_admin_user }}"
          port: 5432
          state: 'present'
```


## Dependencies

None


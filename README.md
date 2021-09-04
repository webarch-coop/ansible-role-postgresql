# Ansible role to install PostgresSQL

This role has been designed to:

* Install PostgresSQL on Debian
* Optionally create a databases and users
* Write the login details of users created to the `postgres` users `$HOME/.pgpass` file
* Create a cronjob to dump all databases on a nightly basis

In the future support for ensuring that an array or dictionary of databases and
users are present might be added.

## Upgrading from Debian Buster to Bullseye

After upgrading the OS PostgreSQL need to be upgraded:

```bash
pg_dropcluster --stop 13 main
pg_upgradecluster 11 main
apt remove postgresql-11 postgresql-client-11
```

## Links

* The Debian [PostgreSQL](https://wiki.debian.org/PostgreSql) wiki page
* The Ansible [postgresql_user](https://docs.ansible.com/ansible/latest/modules/postgresql_user_module.html) module
* The Ansible [postgresql_db](https://docs.ansible.com/ansible/latest/modules/postgresql_db_module.html) modules

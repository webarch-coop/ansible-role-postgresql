# Ansible role to install PostgresSQL

This role has been designed to:

* Install PostgresSQL on Debian
* Create a database and user
* Write the login details to the `postgres` users `$HOME/.pgpass` file

In the future support for ensuring that an array or dictionary of databases and
users are present might be added.

## Links

* The Debian [PostgreSQL](https://wiki.debian.org/PostgreSql) wiki page
* The Ansible [postgresql_user](https://docs.ansible.com/ansible/latest/modules/postgresql_user_module.html) module
* The Ansible [postgresql_db](https://docs.ansible.com/ansible/latest/modules/postgresql_db_module.html) modules

# Webarchitects PostgresSQL Ansible role

[![pipeline status](https://git.coop/webarch/postgresql/badges/master/pipeline.svg)](https://git.coop/webarch/postgresql/-/commits/master)

This role has been designed to:

* Install PostgreSQL on Debian.
* Optionally create a database and a user.
* Write the users password to the PostgreSQL superuser `$HOME/.pgpass` file when a user is added.
* Create a cron job to dump all databases on a nightly basis.

To create multiple users and databases include this role once for each users / database required.

This role can be included using the tasks from the [tasks/pgpass_read.yml](tasks/pgpass_read.yml) file in order to read a users password.

## Requirements

This role requires [JC](https://github.com/kellyjonbrazil/jc) version [1.22.3](https://github.com/kellyjonbrazil/jc/releases/tag/v1.22.3) to be installed on the Ansible controller as the [PostgreSQL password file parser](https://kellyjonbrazil.github.io/jc/docs/parsers/pgpass) is used to read passwords from `/var/lib/postgresql/.pgpass` and the [ini file parser](https://kellyjonbrazil.github.io/jc/docs/parsers/ini) is used to parse the `postgresql.conf` file.

## Defaults

See the [defaults/main.yml](defaults/main.yml) file and the [meta/argument_specs.yml](meta/argument_specs.yml) file.

### postgresql

A boolean that defaults to `false`, set `postgresql` to `true` for the tasks in this role to be run.

### postgresql_backups

The path for the nightly database backups to be written to, `postgresql_backups` defaults to `/var/backups/postgres`.

If this variable is empty then the database backups cron job will not be created.

### postgresql_cron_hour

The hour during which the database backups will be created, the minute is set randomly and saved and read from the PostgreSQL superuser `$HOME/.cron_min` file.

If `postgresql_cron_hour` is empty then the cron job will not be created.

### postgresql_db

The name of a database to be created, by default `postgresql_db` is not defined.

### postgresql_db_encoding

The encoding of the database to be created, for example, `UTF8`, by default `postgresql_db_encoding` is not defined.

### postgresql_db_lc_collate

The database collation order (LC_COLLATE), by default `postgresql_db_lc_collate` is not defined.

### postgresql_db_lc_ctype

The database character classification (LC_CTYPE), by default `postgresql_db_lc_ctype` is not defined.

### postgresql_db_locale

The database locale, if `postgresql_db_locale` is defined and `postgresql_db_lc_collate` and `postgresql_db_lc_ctype` are not then the value of `postgresql_db_locale` will be used for both when creating a database, for example `C`.

### postgresql_db_owner

The, optional, database owner, if not set it will default to `postgres`.

### postgresql_db_state

The state of the database, see [the module parameter](https://docs.ansible.com/ansible/latest/collections/community/postgresql/postgresql_db_module.html#parameter-state), if not set `postgresql_db_state` defaults to `present`.

### postgresql_host

The PostgreSQL host, this variables is required and it defaults to `localhost`.

### postgresql_maintenance_db

The initial PostgreSQL database (which is also called as maintenance DB) that Ansible connects to, it defaults to `postgres`.

### postgresql_pkgs

A list of Debian packages to be installed by this role.

### postgresql_port

The port PostgreSQL uses, `postgresql_port` defaults to `5432`, if `postgresql_port` is set to a value that differs from the port PostgreSQL is using then this role will update the port PostgreSQL uses.

### postgresql_superuser

The PostgreSQL super account, this is set to `postgres` by Debian and probably can't be changed.

### postgresql_user

The name of a PostgreSQL user account to create, `postgresql_user` is not defined by default.

### postgresql_user_privs

The database privileges that the `postgresql_user` should have on the `postgresql_db`, if this variable is not set then `ALL` will be used when adding a database and user.

### postgresql_validate

A boolean, validate variabels that start with `postgresql_` using the [meta/argument_specs.yml](meta/argument_specs.yml) file.

## Upgrading Debian versions

After upgrading Debian PostgreSQL needs to be upgraded, for example when going from Bullseye to Bookworm:

```bash
pg_dropcluster --stop 15 main
pg_upgradecluster 13 main
apt remove postgresql-13 postgresql-client-13
```

And Buster to Bullseye:

```bash
pg_dropcluster --stop 13 main
pg_upgradecluster 11 main
apt remove postgresql-11 postgresql-client-11
```

Ubuntu 20.04 to 22.04:

```bash
pg_dropcluster --stop 12 main
pg_upgradecluster 14 main
apt remove postgresql-12 postgresql-client-12
```

Ubuntu 22.04 to 24.04:

```bash
pg_dropcluster --stop 14 main
pg_upgradecluster 16 main
apt remove postgresql-14 postgresql-client-14
```

The upgrade will also change the port number that PostgreSQL runs on so that both the previous version and the new version can run at the same time, this role can be used to revert the port number after the upgrade.

## References

* The Debian [PostgreSQL](https://wiki.debian.org/PostgreSql) wiki page.
* The Ansible [community.postgresql modules](https://docs.ansible.com/ansible/latest/collections/community/postgresql/index.html).
* The JC [pgpass parser](https://kellyjonbrazil.github.io/jc/docs/parsers/pgpass)

## Repository

The primary URL of this repo is [`https://git.coop/webarch/postgresql`](https://git.coop/webarch/postgresql) however it is also [mirrored to GitHub](https://github.com/webarch-coop/ansible-role-postgresql) and [available via Ansible Galaxy](https://galaxy.ansible.com/chriscroome/postgresql).
If you use this role please use a tagged release, see [the release notes](https://git.coop/webarch/postgresql/-/releases).

## Copyright

Copyright 2020-2025 Chris Croome, &lt;[chris@webarchitects.co.uk](mailto:chris@webarchitects.co.uk)&gt;.

This role is released under [the same terms as Ansible itself](https://github.com/ansible/ansible/blob/devel/COPYING), the [GNU GPLv3](LICENSE).

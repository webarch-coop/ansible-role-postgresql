# Ansible role to install PostgresSQL

If the `postgresql_user` and `postgresql_db` variables are defined then a user
and database are created and a random password for the account is written to
`/root/postgresql.{{ postgresql_user }}.passwd` file.

This role uses the
[postgresql_user](https://docs.ansible.com/ansible/latest/modules/postgresql_user_module.html)
and
[postgresql_db](https://docs.ansible.com/ansible/latest/modules/postgresql_db_module.html)
Ansible modules.

sys_fs.mysql
============

This role installs and configures MySQL from the official repo on Debian and
Ubuntu.

Requirements
------------

This role requires at least Ansible 2.6. Earlier releases may work, but are not
supported.

Role Variables
--------------

	mysql_packages:
	  - mysql-community-server
      - python-mysqldb

Packages installed by the role.

	mysql_version: 5.7

The version of MySQL to install.

	mysql_root_password: hunter2

The root user's password. Encrypt this with `ansible-vault`!

    mysql_bind_address: 127.0.0.1
    mysql_port: 3306

The address and port to listen on.

    mysql_sql_mode: ''

Changes MySQL's behaviour, e.g. making it act more like postgres. See the
[documentation](https://dev.mysql.com/doc/refman/5.7/en/sql-mode.html).

	mysql_innodb_buffer_pool_size: 1G
	mysql_innodb_log_file_size: 256M
	mysql_innodb_flush_log_at_trx_commit: 1
	mysql_innodb_flush_method: O_DIRECT

Settings for innodb.

    mysql_server_id: 1
    mysql_expire_logs_days: 7
    mysql_max_binlog_size: 100M

Settings used to configure the binlog. See the
[documentation](https://dev.mysql.com/doc/refman/5.7/en/replication-options-binary-log.html).

	mysql_extra_variables: []
	#  - name: example
	#    value: 10M

Add arbitrary values to the MySQL config.

	mysql_databases: []
	#  - name: example
	#    encoding: utf8
	#    collation: utf8_general_ci
	#    binlog: True

List of databases to create. The example values of `encoding`, `collation`, and
`binlog` shown are the defaults.

	mysql_users: []
	#  - name: example
	#    host: 127.0.0.1
	#    password: hunter2
	#    priv: 'example.*:ALL'

List of users to create. The example value of `host` shown is the default.

Example Playbook
----------------

    - hosts: databases
	  vars:
	    - mysql_extra_variables:
		    - name: max_allowed_packet
			  value: 128M
		  mysql_databases:
		    - name: foo
		  mysql_users:
		    - name: bar
			  password: hunter2
			  priv: 'foo.*:ALL'
      roles:
        - sys_fs.mysql

License
-------

MIT

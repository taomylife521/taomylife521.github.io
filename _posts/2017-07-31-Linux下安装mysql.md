#环境准备
1. mysql下载地址:https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.19-linux-glibc2.12-x86_64.tar.gz
2. Linux系统版本:centos 7

#安装步骤
[https://dev.mysql.com/doc/refman/5.7/en/binary-installation.html](https://dev.mysql.com/doc/refman/5.7/en/binary-installation.html)
    
    shell> groupadd mysql
	shell> useradd -r -g mysql -s /bin/false mysql
	shell> cd /usr/local
	shell> tar zxvf /path/to/mysql-VERSION-OS.tar.gz
	shell> ln -s full-path-to-mysql-VERSION-OS mysql
	shell> cd mysql
	shell> mkdir mysql-files
	shell> chmod 750 mysql-files
	shell> chown -R mysql .
	shell> chgrp -R mysql .
	shell> bin/mysql_install_db --user=mysql    # MySQL 5.7.5
	shell> bin/mysqld --initialize --user=mysql # MySQL 5.7.6 and up
	shell> bin/mysql_ssl_rsa_setup              # MySQL 5.7.6 and up
	shell> chown -R root .
	shell> chown -R mysql data mysql-files
	shell> bin/mysqld_safe --user=mysql &
	# Next command is optional
	shell> cp support-files/mysql.server /etc/init.d/mysqld
	shell> service mysqld start

#测试服务器是否安装成功
到这步服务也起来了。。但是有个问题就是，我没有设置密码和用户啊。这测试服务器可怎么整？
##通过启动bin/mysql_safe 来启动并修改密码##
1.停掉mysql服务器
	
	[root@DB-Server init.d]# /etc/rc.d/init.d/mysql stop
	Shutting down MySQL..[ OK ]
2.启动mysqld_safe 指定 --skip-grant-tables

    [root@DB-Server init.d]# mysqld_safe --user=mysql --skip-grant-tables --skip-networking &
3.登录mysql服务器

    [root@DB-Server bin]# ./mysql -u[你的用户名] -p[你的密码]

4.修改mysql root密码


	shell>use mysql;
	shell>update user set password=PASSWORD('[你的新密码]') where user='root' and host='root' or host='localhost';
	shell>flush privileges;

5.设置mysql可以远程访问
	
	shell>user mysql;
	shell>GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '[你的root密码]' WITH GRANT OPTION;
	shell>flush privileges;

	  
6.mysql数据库用户密码已修改，现在重新关闭mysqld服务器，然后重新连接下，服务已经正式启动了，接下来运行一下测试shell命令，看看服务器是否运行正常。

- 使用mysqladmin验证服务器是否正在运行。
- 
	shell> bin/mysqladmin version
	shell> bin/mysqladmin variables
如果无法连接到服务器，请指定-u root选项以root身份连接。如果您已经为root帐户分配了密码，则还需要在命令行中指定-p，并在出现提示时输入密码。例如：
	
	shell> bin/mysqladmin -u root -p version
			Enter password: (enter root password here)
根据您的平台和MySQL版本，mysqladmin版本的输出略有不同，但应与之类似：

    shell> bin/mysqladmin version
			mysqladmin  Ver 14.12 Distrib 5.7.20, for pc-linux-gnu on i686
			...
			
			Server version          5.7.20
			Protocol version        10
			Connection              Localhost via UNIX socket
			UNIX socket             /var/lib/mysql/mysql.sock
			Uptime:                 14 days 5 hours 5 min 21 sec
			
			Threads: 1  Questions: 366  Slow queries: 0
			Opens: 0  Flush tables: 1  Open tables: 19
			Queries per second avg: 0.000

要查看还有什么可以使用mysqladmin，请使用--help选项调用它。

##验证关闭服务器##

验证您是否可以关闭服务器（如果root帐户已经有密码，则包括-p选项）

    shell> bin/mysqladmin -u root shutdown
##验证重新开启服务器##
验证您是否可以重新启动服务器。通过使用mysqld_safe或直接调用mysqld来执行此操作。例如：

    shell> bin/mysqld_safe --user=mysql &

如果开启失败，请查阅:[失败原因](https://dev.mysql.com/doc/refman/5.7/en/starting-server-troubleshooting.html)

##查看存在的数据库##
已安装数据库的列表可能有所不同，但始终包含最小的mysql和information_schema.

    shell> bin/mysqlshow
			+--------------------+
			|     Databases      |
			+--------------------+
			| information_schema |
			| mysql              |
			| performance_schema |
			| sys                |
			+--------------------+

如果指定数据库名称，mysqlshow将显示数据库中的表的列表：

    shell> bin/mysqlshow mysql
	Database: mysql
	+---------------------------+
	|          Tables           |
	+---------------------------+
	| columns_priv              |
	| db                        |
	| engine_cost               |
	| event                     |
	| func                      |
	| general_log               |
	| gtid_executed             |
	| help_category             |
	| help_keyword              |
	| help_relation             |
	| help_topic                |
	| innodb_index_stats        |
	| innodb_table_stats        |
	| ndb_binlog_index          |
	| plugin                    |
	| proc                      |
	| procs_priv                |
	| proxies_priv              |
	| server_cost               |
	| servers                   |
	| slave_master_info         |
	| slave_relay_log_info      |
	| slave_worker_info         |
	| slow_log                  |
	| tables_priv               |
	| time_zone                 |
	| time_zone_leap_second     |
	| time_zone_name            |
	| time_zone_transition      |
	| time_zone_transition_type |
	| user                      |
	+---------------------------+

##执行sql命令##

    shell> bin/mysql -e "SELECT User, Host, plugin FROM mysql.user" mysql
	+------+-----------+-----------------------+
	| User | Host      | plugin                |
	+------+-----------+-----------------------+
	| root | localhost | mysql_native_password |
	+------+-----------+-----------------------+



mysql - MySQL命令行工具，

mysqladmin - 用于管理MySQL服务器的客户端

mysqlshow - 显示数据库，表和列信息。
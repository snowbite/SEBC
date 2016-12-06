SLAVE Node
+++++++++++++

Install mysql-server
```
sudo yum install mysql-server
```

Update Server ID and Set Security
```
sudo vi /etc/my.cnf
server-id=2

$ sudo systemctl enable mysqld.service
$ sudo systemctl start mysqld.service
$ sudo systemctl status mysqld
● mysqld.service - MySQL Community Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: active (running) since Tue 2016-12-06 17:11:28 UTC; 8s ago
  Process: 31172 ExecStartPost=/usr/bin/mysql-systemd-start post (code=exited, status=0/SUCCESS)
  Process: 31078 ExecStartPre=/usr/bin/mysql-systemd-start pre (code=exited, status=0/SUCCESS)
 Main PID: 31170 (mysqld_safe)
   CGroup: /system.slice/mysqld.service
           ├─31170 /bin/sh /usr/bin/mysqld_safe
           └─31335 /usr/sbin/mysqld --basedir=/usr --datadir=/var/lib/mysql -...

Dec 06 17:11:27 ip-172-31-13-99.ap-southeast-1.compute.internal mysql-systemd-start[31078]: ...
Dec 06 17:11:27 ip-172-31-13-99.ap-southeast-1.compute.internal mysql-systemd-start[31078]: ...
Dec 06 17:11:27 ip-172-31-13-99.ap-southeast-1.compute.internal mysql-systemd-start[31078]: ...
Dec 06 17:11:27 ip-172-31-13-99.ap-southeast-1.compute.internal mysql-systemd-start[31078]: ...
Dec 06 17:11:27 ip-172-31-13-99.ap-southeast-1.compute.internal mysql-systemd-start[31078]: ...
Dec 06 17:11:27 ip-172-31-13-99.ap-southeast-1.compute.internal mysql-systemd-start[31078]: ...
Dec 06 17:11:27 ip-172-31-13-99.ap-southeast-1.compute.internal mysql-systemd-start[31078]: ...
Dec 06 17:11:27 ip-172-31-13-99.ap-southeast-1.compute.internal mysqld_safe[31170]: ...
Dec 06 17:11:27 ip-172-31-13-99.ap-southeast-1.compute.internal mysqld_safe[31170]: ...
Dec 06 17:11:28 ip-172-31-13-99.ap-southeast-1.compute.internal systemd[1]: S...
Hint: Some lines were ellipsized, use -l to show in full.


$sudo /usr/bin/mysql_secure_installation
```

MASTER
+++++++++++

Update the Config file
```
sudo vi /etc/my.cnf

log-bin=/var/log/mysql/mysql-bin.log
binlog-do-db=amon
server-id=1

$ sudo mkdir /var/log/mysql
$ sudo chown mysql:mysql /var/log/mysql


$ sudo systemctl stop firewalld

$ sudo service mysql restart
```

Set the Replication

```
$ mysql -u root -p

sql> GRANT REPLICATION SLAVE ON *.* TO 'slave_user'@'%' IDENTIFIED BY 'password';

sql> GRANT ALL ON *.* TO 'slave_user'@'%' IDENTIFIED BY 'password';

sql> SET GLOBAL binlog_format = 'ROW';
sql> FLUSH TABLES WITH READ LOCK;
```

open new terminal to show master status
```
$ mysql -u root -p
sql> SHOW MASTER STATUS;

mysql> SHOW MASTER STATUS;
+------------------+----------+--------------+------------------+-------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+------------------+----------+--------------+------------------+-------------------+
| mysql-bin.000001 |      525 | amon         |                  |                   |
+------------------+----------+--------------+------------------+-------------------+
1 row in set (0.00 sec)

exit
```

Exit original terminal

```
sql> UNLOCK TABLES;
```

SLAVE
+++++++++++++++

Setup the Slave

```
$ sudo systemctl stop firewalld

$ mysql -u root -p

sql> create database amon DEFAULT CHARACTER SET utf8;
sql> grant all on amon.* TO 'amon'@'%' IDENTIFIED BY 'amon_password';

sql> CHANGE MASTER TO MASTER_HOST='172.31.13.100', MASTER_USER='slave_user', MASTER_PASSWORD='password', MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=525;

sql> START SLAVE;

sql> SHOW SLAVE STATUS \G



mysql> SHOW SLAVE STATUS \G
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 172.31.13.100
                  Master_User: slave_user
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000001
          Read_Master_Log_Pos: 525
               Relay_Log_File: mysqld-relay-bin.000002
                Relay_Log_Pos: 283
        Relay_Master_Log_File: mysql-bin.000001
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB:
          Replicate_Ignore_DB:
           Replicate_Do_Table:
       Replicate_Ignore_Table:
      Replicate_Wild_Do_Table:
  Replicate_Wild_Ignore_Table:
                   Last_Errno: 0
                   Last_Error:
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 525
              Relay_Log_Space: 457
              Until_Condition: None
               Until_Log_File:
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File:
           Master_SSL_CA_Path:
              Master_SSL_Cert:
            Master_SSL_Cipher:
               Master_SSL_Key:
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error:
               Last_SQL_Errno: 0
               Last_SQL_Error:
  Replicate_Ignore_Server_Ids:
             Master_Server_Id: 1
                  Master_UUID: 407198e7-bb7e-11e6-9978-029618933133
             Master_Info_File: /var/lib/mysql/master.info
                    SQL_Delay: 0
          SQL_Remaining_Delay: NULL
      Slave_SQL_Running_State: Slave has read all relay log; waiting for the slave I/O thread to update it
           Master_Retry_Count: 86400
                  Master_Bind:
      Last_IO_Error_Timestamp:
     Last_SQL_Error_Timestamp:
               Master_SSL_Crl:
           Master_SSL_Crlpath:
           Retrieved_Gtid_Set:
            Executed_Gtid_Set:
                Auto_Position: 0
1 row in set (0.00 sec)
```

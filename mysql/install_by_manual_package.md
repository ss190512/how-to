## Install Oracle Linux 7.3
### Pick the following options of the "Software Selection".
```
Base Environment > Server with GUI
Add-Ons for Selected Environment > Compatibility Libraries
Add-Ons for Selected Environment > Development Tools
```
### Change Secure Linux (SELinux) to disabled or permissive by edit the "/etc/selinux/config" file.
```
SELINUX=permissive
```
### Disable the firewall(run as root).
```
systemctl stop firewalld
systemctl disable firewalld
systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
     Docs: man:firewalld(1)
```
### Make sure the SSH daemon is started(run as root).
```
systemctl start sshd.service
systemctl enable sshd.service
systemctl status sshd.service
● sshd.service - OpenSSH server daemon
   Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2019-05-13 19:30:06 EDT; 5 days ago
...
```
### Change the host name(run as root).
```
nmcli general hostname
localhost.localdomain
nmcli general hostname ol73ms
nmcli general hostname
ol73ms
```
### Install needed libraries but they should be already installed(run as root).
```
yum search libaio
yum install libaio
```
### Reboot.
## Download the package
```
https://dev.mysql.com/downloads/mysql/
mysql-8.0.16-linux-glibc2.12-x86_64.tar.xz
```
## Prepare for installation. Run as root.
```
cd /usr/local/
mv /root/Downloads/mysql-8.0.16-linux-glibc2.12-x86_64.tar.xz ./
tar -xf mysql-8.0.16-linux-glibc2.12-x86_64.tar.xz
groupadd mysql
useradd -r -g mysql -s /bin/false mysql
ln -s mysql-8.0.16-linux-glibc2.12-x86_64 ./mysql
cd mysql
pwd
/usr/local/mysql
mkdir mysql-files
chmod 750 ./mysql-files/ 
chown -R mysql:mysql /usr/local/mysql/
```
## Generate root password.
```
# bin/mysqld --initialize --user=mysql
2019-05-14T02:50:53.535309Z 0 [Warning] [MY-011070] [Server] 'Disabling symbolic links using --skip-symbolic-links (or equivalent) is the default. Consider not using this option as it' is deprecated and will be removed in a future release.
2019-05-14T02:50:53.535888Z 0 [System] [MY-013169] [Server] /usr/local/mysql-8.0.16-linux-glibc2.12-x86_64/bin/mysqld (mysqld 8.0.16) initializing of server in progress as process 20529
2019-05-14T02:50:54.919123Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: iu#6FxXsFt6h
2019-05-14T02:50:55.559995Z 0 [System] [MY-013170] [Server] /usr/local/mysql-8.0.16-linux-glibc2.12-x86_64/bin/mysqld (mysqld 8.0.16) initializing of server has completed
```
## Generate SSL keys.
```
# bin/mysql_ssl_rsa_setup
```
## Change permissions back to root.
```
# chown -R root .
# chown -R mysql data/ mysql-files/
```
## Run the server in the background.
```
# bin/mysqld_safe --user=mysql &
[1] 3452
[root@ol73ms mysql]# Logging to '/usr/local/mysql/data/ol73ms.err'.
2019-05-21T01:18:59.831730Z mysqld_safe Starting mysqld daemon with databases from /usr/local/mysql/data
```
## Login using previously created password.
```
# bin/mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.16
Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> show databases;
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
mysql> alter user 'root'@'localhost' identified by 'newPassword#123';
Query OK, 0 rows affected (0.00 sec)
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.01 sec)
mysql>
```
## Create auto-start script.
```
# pwd
/usr/local/mysql
# cp support-files/mysql.server /etc/init.d/mysql.server
# /etc/init.d/mysql.server restart
Shutting down MySQL...2019-05-04T02:48:45.472221Z mysqld_safe mysqld from pid file /usr/local/mysql/data/ol73ms.pid ended
 SUCCESS! 
Starting MySQL. SUCCESS! 
[1]+  Done                    bin/mysqld_safe --user=mysql
# /etc/init.d/mysql.server status
 SUCCESS! MySQL running (8669)
```
## Add mysql into path.
```
# pwd
/root
# grep PATH .bashrc
export PATH=$PATH:/usr/local/mysql/bin

```

## Install Oracle Linux 7.6
### Pick the following options of the "Software Selection".
```
Base Environment > Server with GUI
Add-Ons for Selected Environment > Compatibility Libraries
Add-Ons for Selected Environment > Development Tools
```
### Change Secure Linux (SELinux) to disabled or permissive by edit the "/etc/selinux/config" file.
```
SELINUX=permissive

[root@ol76odb18 ~]# setenforce Permissive
```
### Disable the firewall(run as root).
```
systemctl stop firewalld
systemctl disable firewalld
systemctl status firewalld
? firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
     Docs: man:firewalld(1)
```
### Make sure the SSH daemon is started(run as root).
```
systemctl start sshd.service
systemctl enable sshd.service
systemctl status sshd.service
? sshd.service - OpenSSH server daemon
   Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2019-05-13 19:30:06 EDT; 5 days ago
...
```
### Change the host name(run as root).
```
nmcli general hostname
localhost.localdomain
nmcli general hostname ol76odb18.localdomain
nmcli general hostname
ol76odb18.localdomain
```
### Check names in OS files.
```
[root@ol76odb18 ~]# cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.1.21 ol76odb18.localdomain ol76odb18

[root@ol76odb18 ~]# cat /etc/hostname
ol76odb18.localdomain
```
## Install oracle software.
### Install oracle-database-preinstall-18c package.
```
[root@ol76odb18 ~]# yum install -y oracle-database-preinstall-18c
Loaded plugins: langpacks, ulninfo
Resolving Dependencies
--> Running transaction check
---> Package oracle-database-preinstall-18c.x86_64 0:1.0-1.el7 will be installed
--> Processing Dependency: ksh for package: oracle-database-preinstall-18c-1.0-1.el7.x86_64
--> Processing Dependency: libaio-devel for package: oracle-database-preinstall-18c-1.0-1.el7.x86_64
--> Running transaction check
---> Package ksh.x86_64 0:20120801-139.0.1.el7 will be installed
---> Package libaio-devel.x86_64 0:0.3.109-13.el7 will be installed
--> Finished Dependency Resolution
Dependencies Resolved
...
Installed:
  oracle-database-preinstall-18c.x86_64 0:1.0-1.el7
Dependency Installed:
  ksh.x86_64 0:20120801-139.0.1.el7                                                           libaio-devel.x86_64 0:0.3.109-13.el7
Complete!
[root@ol76odb18 ~]#
```
### Change oracle user password.
```
[root@ol76odb18 ~]# passwd oracle
Changing password for user oracle.
New password:
Retype new password:
passwd: all authentication tokens updated successfully.
```
### Create directories. 
```
mkdir -p /u01/app/oracle/product/18.3/dbhome_1
mkdir -p /u02/oradata
chown -R oracle:oinstall /u01 /u02
chmod -R 775 /u01 /u02
```
### Create oracle scripts. 
```
[oracle@ol76odb18 scripts]$ pwd
/home/oracle/scripts
[oracle@ol76odb18 scripts]$ cat set_ora_env.sh
export TMP=/tmp
export TMPDIR=$TMP
export ORACLE_HOSTNAME=ol76odb18.localdomain
export ORACLE_UNQNAME=db1
export ORACLE_BASE=/u01/app/oracle
export ORACLE_HOME=$ORACLE_BASE/product/18.3/dbhome_1
export ORA_INVENTORY=/u01/app/oraInventory
export ORACLE_SID=db1
export DATA_DIR=/u02/oradata
export PATH=/usr/sbin:/usr/local/bin:$PATH
export PATH=$ORACLE_HOME/bin:$PATH
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib
export CLASSPATH=$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib
[oracle@ol76odb18 scripts]$

[oracle@ol76odb18 ~]$ cat .bash_profile
...
. /home/oracle/scripts/set_ora_env.sh
[oracle@ol76odb18 ~]$

[oracle@ol76odb18 scripts]$ cat start_ora.sh
#!/bin/bash
. /home/oracle/scripts/set_ora_env.sh
export ORAENV_ASK=NO
. oraenv
export ORAENV_ASK=YES
dbstart $ORACLE_HOME

[oracle@ol76odb18 scripts]$ cat stop_ora.sh
#!/bin/bash
. /home/oracle/scripts/set_ora_env.sh
export ORAENV_ASK=NO
. oraenv
export ORAENV_ASK=YES
dbshut $ORACLE_HOME

[oracle@ol76odb18 scripts]$ chown -R oracle:oinstall /home/oracle/scripts
[oracle@ol76odb18 scripts]$ chmod u+x /home/oracle/scripts/*.sh

```
### Run X-server for example MobaXterm.
### Unzip oracle software.
```
[oracle@ol76odb18 scripts]$ cd $ORACLE_HOME
[oracle@ol76odb18 dbhome_1]$ pwd
/u01/app/oracle/product/18.3/dbhome_1
[oracle@ol76odb18 dbhome_1]$ unzip -oq /home/oracle/download/LINUX.X64_180000_db_home.zip
```


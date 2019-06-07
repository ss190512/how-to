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
### Check memory which should be at least 8Gb.
```
[oracle@ol76odb18 ~]$ free --mega
              total        used        free      shared  buff/cache   available
Mem:           8910         316        8171          11         422        8427
Swap:          3967           0        3967
[oracle@ol76odb18 ~]$
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
### Run X-server(for example MobaXterm).
### Unzip oracle software.
```
[oracle@ol76odb18 scripts]$ cd $ORACLE_HOME
[oracle@ol76odb18 dbhome_1]$ pwd
/u01/app/oracle/product/18.3/dbhome_1
[oracle@ol76odb18 dbhome_1]$ unzip -oq /home/oracle/download/LINUX.X64_180000_db_home.zip
```
### Run the installer.
```
# Silent mode.
[oracle@ol76odb18 dbhome_1]$ pwd
/u01/app/oracle/product/18.3/dbhome_1
[oracle@ol76odb18 dbhome_1]$ ./runInstaller -ignorePrereq -waitforcompletion -silent                        \
    -responseFile ${ORACLE_HOME}/install/response/db_install.rsp\
    oracle.install.option=INSTALL_DB_SWONLY\
    ORACLE_HOSTNAME=${ORACLE_HOSTNAME}\
    UNIX_GROUP_NAME=oinstall\
    INVENTORY_LOCATION=${ORA_INVENTORY}\
    SELECTED_LANGUAGES=en\
    ORACLE_HOME=${ORACLE_HOME}\
    ORACLE_BASE=${ORACLE_BASE}\
    oracle.install.db.InstallEdition=EE\
    oracle.install.db.OSDBA_GROUP=dba\
    oracle.install.db.OSBACKUPDBA_GROUP=dba\
    oracle.install.db.OSDGDBA_GROUP=dba\
    oracle.install.db.OSKMDBA_GROUP=dba\
    oracle.install.db.OSRACDBA_GROUP=dba\
    SECURITY_UPDATES_VIA_MYORACLESUPPORT=false\
    DECLINE_SECURITY_UPDATES=true

Launching Oracle Database Setup Wizard...
[WARNING] [INS-13014] Target environment does not meet some optional requirements.
   CAUSE: Some of the optional prerequisites are not met. See logs for details. installActions2019-06-06_11-50-13PM.log
   ACTION: Identify the list of failed prerequisite checks from the log: installActions2019-06-06_11-50-13PM.log. Then either from the log file or from installation manual find the appropriate configuration to meet the prerequisites and fix it manually.
The response file for this session can be found at:
 /u01/app/oracle/product/18.3/dbhome_1/install/response/db_2019-06-06_11-50-13PM.rsp
You can find the log of this install session at:
 /tmp/InstallActions2019-06-06_11-50-13PM/installActions2019-06-06_11-50-13PM.log
As a root user, execute the following script(s):
        1. /u01/app/oraInventory/orainstRoot.sh
        2. /u01/app/oracle/product/18.3/dbhome_1/root.sh
Execute /u01/app/oraInventory/orainstRoot.sh on the following nodes:
[ol76odb18]
Execute /u01/app/oracle/product/18.3/dbhome_1/root.sh on the following nodes:
[ol76odb18]
Successfully Setup Software with warning(s).
Moved the install session logs to:
 /u01/app/oraInventory/logs/InstallActions2019-06-06_11-50-13PM
```
### Check warnings.
```
[oracle@ol76odb18 InstallActions2019-06-06_11-50-13PM]$ pwd
/u01/app/oraInventory/logs/InstallActions2019-06-06_11-50-13PM
[oracle@ol76odb18 InstallActions2019-06-06_11-50-13PM]$ less  installActions2019-06-06_11-50-13PM.log
...
WARNING:  [Jun 6, 2019 11:50:24 PM] Result values are not available for this verification task
INFO:  [Jun 6, 2019 11:50:24 PM] Creating CompositePrereqChecker Job for container task User Existence: oracle
INFO:  [Jun 6, 2019 11:50:24 PM] Creating PrereqChecker Job for leaf task Users With Same UID: 54321
INFO:  [Jun 6, 2019 11:50:24 PM] Creating PrereqChecker Job for leaf task Users With Same UID: 54321
INFO:  [Jun 6, 2019 11:50:24 PM] *********************************************
INFO:  [Jun 6, 2019 11:50:24 PM] Group Existence: dba: This is a prerequisite condition to test whether group "dba" exists on the system.
INFO:  [Jun 6, 2019 11:50:24 PM] Severity:CRITICAL
INFO:  [Jun 6, 2019 11:50:24 PM] OverallStatus:SUCCESSFUL
INFO:  [Jun 6, 2019 11:50:24 PM] -----------------------------------------------
INFO:  [Jun 6, 2019 11:50:24 PM] Verification Result for Node:ol76odb18
WARNING:  [Jun 6, 2019 11:50:24 PM] Result values are not available for this verification task
INFO:  [Jun 6, 2019 11:50:24 PM] *********************************************
INFO:  [Jun 6, 2019 11:50:24 PM] Group Existence: oinstall: This is a prerequisite condition to test whether group "oinstall" exists on the system.
INFO:  [Jun 6, 2019 11:50:24 PM] Severity:CRITICAL
INFO:  [Jun 6, 2019 11:50:24 PM] OverallStatus:SUCCESSFUL
INFO:  [Jun 6, 2019 11:50:24 PM] -----------------------------------------------
INFO:  [Jun 6, 2019 11:50:24 PM] Verification Result for Node:ol76odb18
WARNING:  [Jun 6, 2019 11:50:24 PM] Result values are not available for this verification task
INFO:  [Jun 6, 2019 11:50:24 PM] *********************************************
INFO:  [Jun 6, 2019 11:50:24 PM] Group Membership: oinstall(Primary): This is a prerequisite condition to test whether user "oracle" has group "oinstall" as its primary group.
INFO:  [Jun 6, 2019 11:50:24 PM] Severity:CRITICAL
INFO:  [Jun 6, 2019 11:50:24 PM] OverallStatus:SUCCESSFUL
INFO:  [Jun 6, 2019 11:50:24 PM] -----------------------------------------------
INFO:  [Jun 6, 2019 11:50:24 PM] Verification Result for Node:ol76odb18
WARNING:  [Jun 6, 2019 11:50:24 PM] Result values are not available for this verification task
INFO:  [Jun 6, 2019 11:50:24 PM] *********************************************
INFO:  [Jun 6, 2019 11:50:24 PM] Group Membership: dba: This is a prerequisite condition to test whether user "oracle" is a member of the group "dba".
INFO:  [Jun 6, 2019 11:50:24 PM] Severity:CRITICAL
INFO:  [Jun 6, 2019 11:50:24 PM] OverallStatus:SUCCESSFUL
INFO:  [Jun 6, 2019 11:50:24 PM] -----------------------------------------------
INFO:  [Jun 6, 2019 11:50:24 PM] Verification Result for Node:ol76odb18
WARNING:  [Jun 6, 2019 11:50:24 PM] Result values are not available for this verification task
INFO:  [Jun 6, 2019 11:50:24 PM] *********************************************

```
### Run root scripts.
```
[root@ol76odb18 ~]# cd /u01/app/oraInventory
[root@ol76odb18 oraInventory]# ./orainstRoot.sh
Changing permissions of /u01/app/oraInventory.
Adding read,write permissions for group.
Removing read,write,execute permissions for world.
Changing groupname of /u01/app/oraInventory to oinstall.
The execution of the script is complete.
[root@ol76odb18 oraInventory]# cd /u01/app/oracle/product/18.3/dbhome_1
[root@ol76odb18 dbhome_1]# ./root.sh
Check /u01/app/oracle/product/18.3/dbhome_1/install/root_ol76odb18.localdomain_2019-06-07_00-00-48-655715599.log for the output of root script
[root@ol76odb18 dbhome_1]#
```
## Create a data-base.
### Check oratab file.
```
[oracle@ol76odb18 ~]$ cat /etc/oratab
#db1:/u01/app/oracle/product/18.3/dbhome_1:Y
```
### Using oracle managed files(OMF).
```
[oracle@ol76odb18 bin]$ pwd
/u01/app/oracle/product/18.3/dbhome_1/bin

[oracle@ol76odb18 bin]$ dbca -silent -createDatabase \
 -templateName General_Purpose.dbc \
 -gdbname db1 -sid db1 -responseFile NO_VALUE \
 -characterSet AL32UTF8 \
 -sysPassword myPasswd#123 \
 -systemPassword myPasswd#123 \
 -createAsContainerDatabase false \
 -pdbAdminPassword myPasswd#123 \
 -databaseType MULTIPURPOSE \
 -automaticMemoryManagement false \
 -totalMemory 2048 \
 -storageType FS \
 -datafileDestination "/u02/oradata/" \
 -useOMF true \
 -redoLogFileSize 100 \
 -emConfiguration NONE \
 -ignorePreReqs

Prepare for db operation
10% complete
Copying database files
40% complete
Creating and starting Oracle instance
42% complete
46% complete
50% complete
54% complete
60% complete
Completing Database Creation
66% complete
69% complete
70% complete
Executing Post Configuration Actions
100% complete
Database creation complete. For details check the logfiles at:
 /u01/app/oracle/cfgtoollogs/dbca/db1.
Database Information:
Global Database Name:db1
System Identifier(SID):db1
Look at the log file "/u01/app/oracle/cfgtoollogs/dbca/db1/db1.log" for further details.

[oracle@ol76odb18 bin]$ sqlplus /nolog
SQL*Plus: Release 18.0.0.0.0 - Production on Fri Jun 7 00:52:05 2019
Version 18.3.0.0.0
Copyright (c) 1982, 2018, Oracle.  All rights reserved.
SQL> conn / as sysdba
Connected.
SQL> select * from global_name;
GLOBAL_NAME
--------------------------------------------------------------------------------
DB1
```


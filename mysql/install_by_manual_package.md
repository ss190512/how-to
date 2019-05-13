## Install Oracle Linux 7.6
### Pick the following options of the "Software Selection".
```
Base Environment > Server with GUI
Add-Ons for Selected Environment > Compatibility Libraries
Add-Ons for Selected Environment > Development Tools
Add-Ons for Selected Environment > System Administration Tools
```
### Change Secure Linux (SELinux) to disabled or permissive by edit the "/etc/selinux/config" file.
```
SELINUX=permissive
```
### Disable the firewall.
```
# systemctl stop firewalld
# systemctl disable firewalld
```
### Make sure the SSH daemon is started.
```
# systemctl start sshd.service
# systemctl enable sshd.service
```
### Change the host name.
```
[root@localhost ~]# nmcli general hostname
localhost.localdomain
[root@localhost ~]# nmcli general hostname ol76ms
[root@localhost ~]# nmcli general hostname
ol76ms
```
### Install needed libraries but they should be already installed.
```
# yum search libaio
# yum install libaio
```
### Reboot.


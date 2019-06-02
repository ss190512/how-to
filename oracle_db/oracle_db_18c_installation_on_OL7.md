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
nmcli general hostname ol76odb18 
nmcli general hostname
ol76odb18
```

# 11g

| SHA-256                                                          | File                                   |
| -----------------------------------------------------------------| ---------------------------------      |
| 0B399A6593804C04B4BD65F61E73575341A49F8A273ACABA0DCDA2DFEC4979E0 | p13390677_112040_Linux-x86-64_1of7.zip |
| 73E04957EE0BF6F3B3E6CFCF659BDF647800FE52A377FB8521BA7E3105CCC8DD | p13390677_112040_Linux-x86-64_2of7.zip |

[Hướng dẫn cài đặt bằng Tiếng Việt](https://docs.google.com/document/preview?id=1BPYx0ER6g2-bD075CPFRav4uTyE5J2UCwcl3yIqHijk)

# [**Oracle Database Preinstallation Requirements**](https://docs.oracle.com/cd/E24697_01/doc/install.11203/e24321/pre_install.htm)

Kiểm tra và cài đặt các package thiếu

```console
rpm -q --qf '%{NAME}-%{VERSION}-%{RELEASE}(%{ARCH})\n' binutils \
compat-libstdc++-33 \
elfutils-libelf \
elfutils-libelf-devel \
gcc \
gcc-c++ \
glibc \
glibc-common \
glibc-devel \
glibc-headers \
kernel-headers \
ksh \
libaio \
libaio-devel \
libgcc \
libgomp \
libstdc++ \
libstdc++-devel \
make \
sysstat

#binutils-2.27-44.base.0.1.el7(x86_64)
package compat-libstdc++-33 is not installed
#elfutils-libelf-0.176-5.el7(x86_64)
package elfutils-libelf-devel is not installed
package gcc is not installed
package gcc-c++ is not installed
#glibc-2.17-317.0.1.el7(x86_64)
#glibc-common-2.17-317.0.1.el7(x86_64)
package glibc-devel is not installed
package glibc-headers is not installed
package kernel-headers is not installed
package ksh is not installed
#libaio-0.3.109-13.el7(x86_64)
package libaio-devel is not installed
#libgcc-4.8.5-44.0.3.el7(x86_64)
#libgomp-4.8.5-44.0.3.el7(x86_64)
#libstdc++-4.8.5-44.0.3.el7(x86_64)
package libstdc++-devel is not installed
#make-3.82-24.el7(x86_64)
#sysstat-10.1.5-19.el7(x86_64)

yum install compat-libstdc++-33 \
elfutils-libelf-devel \
gcc \
gcc-c++ \
glibc-devel \
glibc-headers \
kernel-headers \
ksh \
libaio-devel \
libstdc++-devel
```

## 11g tren OL7U9
```console
rpm -Uvh kernel-headers-3.10.0-1160.el7.x86_64.rpm
rpm -Uvh glibc-headers-2.17-317.0.1.el7.x86_64.rpm
rpm -Uvh glibc-devel-2.17-317.0.1.el7.x86_64.rpm
rpm -Uvh cpp-4.8.5-44.0.3.el7.x86_64.rpm
rpm -Uvh libaio-devel-0.3.109-13.el7.x86_64.rpm
rpm -Uvh compat-libstdc++-33-3.2.3-72.el7.x86_64.rpm
rpm -Uvh zlib-devel-1.2.7-18.el7.x86_64.rpm
rpm -Uvh elfutils-libelf-devel-0.176-5.el7.x86_64.rpm
rpm -Uvh libstdc++-devel-4.8.5-44.0.3.el7.x86_64.rpm
rpm -Uvh gcc-4.8.5-44.0.3.el7.x86_64.rpm
rpm -Uvh gcc-c++-4.8.5-44.0.3.el7.x86_64.rpm

```
Set the system host name
```console
sudo hostnamectl set-hostname myhost.mydomain
```

Hosts File
```console
[root@svr01 ~]# vi /etc/hosts

# <IP-address>  <fully-qualified-machine-name>  <machine-name>
192.168.140.130 svr01 svr01.ol7u9
```
Manual Setup
```console
[root@svr01 ~]# vi /etc/sysctl.conf

fs.aio-max-nr = 1048576
fs.file-max = 6815744
kernel.shmall = 2097152
kernel.shmmax = 4294967295
kernel.shmmni = 4096
# semaphores: semmsl, semmns, semopm, semmni
kernel.sem = 250 32000 100 128
net.ipv4.ip_local_port_range = 9000 65500
net.core.rmem_default=262144
net.core.rmem_max=4194304
net.core.wmem_default=262144
net.core.wmem_max=1048586
```

Run the following command to change the current kernel parameters.
```console
[root@svr01 ~]# /sbin/sysctl -p
```

Add the following lines to the "/etc/security/limits.conf" file.
```console
[root@svr01 ~]# vi /etc/security/limits.conf

oracle              soft    nproc   2047
oracle              hard    nproc   16384
oracle              soft    nofile  4096
oracle              hard    nofile  65536
oracle              soft    stack   10240
```

Add the following line to the "/etc/pam.d/login" file, if it does not already exist.
```console
[root@svr01 ~]# vi /etc/pam.d/login
session    required     pam_limits.so
```

Create the new groups and users.
```console
groupadd -g 54321 oinstall
groupadd -g 54322 dba
groupadd -g 54323 oper
#groupadd -g 54324 backupdba
#groupadd -g 54325 dgdba
#groupadd -g 54326 kmdba
#groupadd -g 54327 asmdba
#groupadd -g 54328 asmoper
#groupadd -g 54329 asmadmin

useradd -g oinstall -G dba,oper oracle
```

# Additional Setup

Set the password for the "oracle" user.
```console
[root@svr01 ~]# passwd oracle
```
Set secure Linux to permissive by editing the "/etc/selinux/config" file, making sure the SELINUX flag is set as follows.
```console
SELINUX=permissive
```
Once the change is complete, restart the server or run the following command.
```console
# setenforce Permissive
```
If you have the Linux firewall enabled, you will need to disable or configure it, as shown here or here. To disable it, do the following.
```console
# systemctl stop firewalld
# systemctl disable firewalld
```
Create the directories in which the Oracle software will be installed.
```console
mkdir -p /u01/app/oracle/product/11.2.0.4/db_1
chown -R oracle:oinstall /u01
chmod -R 775 /u01

# Data
mkdir -p /data
chown -R oracle:oinstall /data
chmod -R 775 /data
```

Add the following lines at the end of the "/home/oracle/.bash_profile" file.
```console
[root@svr01 /]# vi /home/oracle/.bash_profile

# Oracle Settings
TMP=/tmp; export TMP
TMPDIR=$TMP; export TMPDIR

ORACLE_HOSTNAME=svr01.ol7u9; export ORACLE_HOSTNAME
ORACLE_UNQNAME=DB11G; export ORACLE_UNQNAME
ORACLE_BASE=/u01/app/oracle; export ORACLE_BASE
ORACLE_HOME=$ORACLE_BASE/product/11.2.0.4/db_1; export ORACLE_HOME
ORACLE_SID=DB11G; export ORACLE_SID
ORACLE_TERM=xterm; export ORACLE_TERM
PATH=/usr/sbin:$PATH; export PATH
PATH=$ORACLE_HOME/bin:$PATH; export PATH

LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib; export LD_LIBRARY_PATH
CLASSPATH=$ORACLE_HOME/JRE:$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib; export CLASSPATH

```

## Hardware Requirements
### Memory Requirements
### System Architecture
### Disk Space Requirements


## Disk partition
Boot 
Swap 
/u01
/root



# 19c

Post Install
- Kiểm tra và tắt process SYS_AUTO_STS_MODULE (tự động phân tích và tạo index -> gây tải cao cho hệ thống)

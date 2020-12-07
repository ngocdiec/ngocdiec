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

## Hardware Requirements
### Memory Requirements
### System Architecture
### Disk Space Requirements


## Disk partition
Boot 
Swap 
/u01
/root

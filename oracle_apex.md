
# Installation

# Configuration

Chuẩn bị dữ liệu:
	- Dump dữ liệu SMS (phân hoạch theo tháng) sang schema trên máy chủ 10.22.2.101
	- Loại bỏ các cột không cần thiết để tối ưu về mặt lưu trữ
	- Tạo job đưa dữ liệu sang schema này hằng ngày
	- Tạo job xóa dữ liệu trên schema này theo tháng (chạy hằng tháng vào ngày đầu tháng)

Xin máy chủ
	- Cài đặt Oracle 11g
	- Cài Tomcat (https://oracle-base.com/articles/linux/apache-tomcat-9-installation-on-linux)
	- Mở kết nối đến 10.22.2.101, port 1521
	- Cài đặt apex 20.2 
	

https://docs.oracle.com/en/database/oracle/application-express/20.2/htmig/downloading-installing-apex.html#GUID-7E432C6D-CECC-4977-B183-3C654380F7BF





@apxsilentins.sql tablespace_apex tablespace_files tablespace_temp images
      password_apex_pub_user password_apex_listener password_apex_rest_pub_user
      password_internal_admin
      
@apxsilentins.sql SYSAUX SYSAUX TEMP /i/ Passw0rd!1 Passw0rd!2 Passw0rd!3 Passw0rd!4      

-- Kiem tra Oracle XML DB HTTP Server - neu bang 0 la disable
SELECT DBMS_XDB.GETHTTPPORT FROM DUAL;

-- Enable Oracle XML DB HTTP Server
exec dbms_xdb.sethttpport('8081');
commit;

--The structure of the link to the Application Express administration services is as follows:
--http://host:port/ords/apex_admin

--The structure of the link to the Application Express development interface is as follows:
--http://host:port/ords


-- Bang luu thong tin user dang nhap APEX
WWV_FLOW_FND_USER


@apex_rest_config.sql Passw0rd!2 Passw0rd!2






-------------------------------------------------------------------------------------------



https://www.youtube.com/watch?v=qDGhzu9tePs




Cài APEX 20.2 không cần Tomcat



ords yêu cầu jkd8

cần mở kết nối ACL

cần unlock một số user liên quan đến 
	- APEX: APEX_PUBLIC_USER, APEX_LISTENER, APEX_REST_PUBLIC_USER
	- ORDS: ORDS_PUBLIC_USER

cần copy thư mục images trong apxex sang ords

tạo 1 thư mục config trong ords để lưu config


-------------------------------------------------------------------------------------------
DEPLOY APPLICATION TO END USER


https://docs.oracle.com/cd/B28359_01/appdev.111/b28551/deploy_app.htm#BABGJIAD -- old version

https://apex.oracle.com/en/platform/deployment/


-------------------------------------------------------------------------------------------
# Enviroment:
OS   : Oracle Linux 6 Update 8
JRE  : 8
DB   : 11gr2
APEX : 20.2
ORDS : 20.3

Cài đặt JRE 8 nếu OS hiện tại đang cài bản thấp hơn
1. Cài online (nếu có kết nối internet
```console
[root@dbvnpay ~]# yum search openjdk
[root@dbvnpay ~]# yum install java-1.8.0-openjdk.x86_64

```
2. Cài offline nếu không có kết nối internet
Tải các gói sau và copy vào máy chủ

[java-1.8.0-openjdk.x86_64](https://yum.oracle.com/repo/OracleLinux/OL6/8/base/x86_64/getPackage/java-1.8.0-openjdk-1.8.0.91-1.b14.el6.x86_64.rpm)

[java-1.8.0-openjdk-headless.x86_64](https://yum.oracle.com/repo/OracleLinux/OL6/8/base/x86_64/getPackage/java-1.8.0-openjdk-headless-1.8.0.91-1.b14.el6.x86_64.rpm)

```console
yum localinstall java-1.8.0-openjdk-headless-1.8.0.91-1.b14.el6.x86_64.rpm java-1.8.0-openjdk-1.8.0.91-1.b14.el6.x86_64.rpm
```

copy 2 file vào cùng phân vùng cài oracle cà giải nén
- apex_20.2.zip (/u01/apex)
- ords-20.3.0.301.1819.zip (/u01/ords)

```console
[oracle@dbvnpay ~]$ cd /u01
[oracle@dbvnpay u01]$ mkdir ords
[oracle@dbvnpay u01]$ cp -r apex/images/ ords/
[oracle@dbvnpay u01]$ mkdir -p ords/config

/u01
├── apex
├── app
├── ords
│   ├── config
│   ├── docs
│   ├── examples
│   ├── images
│   ├── index.html
│   ├── ords.war
│   ├── params
```


# startup database
```console
[oracle@dbvnpay ~]$ lsnrctl start

[oracle@dbvnpay ~]$ sqlplus / as sysdba

SQL> startup;

SQL> select open_mode from v$database;

OPEN_MODE
--------------------
READ WRITE
```

# Cài đặt APEX

```console
[oracle@dbvnpay ~]$ cd /u01/apex/
[oracle@dbvnpay ~]$ sqlplus / as sysdba
SQL> @apexins.sql sysaux sysaux temp /i/
```
@apexins.sql
Cài đặt APEX


@apxchpwd.sql
Thay đổi thông tin của user quản trị
```console
[oracle@dbvnpay apex]$ pwd
/u01/apex
[oracle@dbvnpay apex]$ sqlplus / as sysdba
SQL> @apxchpwd.sql
================================================================================
This script can be used to change the password of an Application Express
instance administrator. If the user does not yet exist, a user record will be
created.
================================================================================
Enter the administrator's username [ADMIN] 
User "ADMIN" exists.
Enter ADMIN's email [ADMIN] ngoctv@vnpay.vn
Enter ADMIN's password [] 
Changed password of instance administrator ADMIN.

```


@apex_rest_config.sql
aaa


cp -r apex/images/ ords/

cd ords/

mkdir config

ORDS yêu cầu Java 8

# Cài đặt ORDS

java -jar ords.war install






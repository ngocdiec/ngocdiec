
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
- OS   : Oracle Linux 6 Update 8
- JRE  : 8
- DB   : 11gr2
- APEX : 20.2
- ORDS : 20.3

Cài đặt JRE 8 nếu OS hiện tại đang cài bản thấp hơn
1. Cài online (nếu có kết nối internet
```console
[root@dbvnpay ~]# yum search openjdk
[root@dbvnpay ~]# yum install java-1.8.0-openjdk.x86_64

```
2. Cài offline nếu không có kết nối internet
Tải các gói sau và copy vào máy chủ
- [java-1.8.0-openjdk.x86_64](https://yum.oracle.com/repo/OracleLinux/OL6/8/base/x86_64/getPackage/java-1.8.0-openjdk-1.8.0.91-1.b14.el6.x86_64.rpm)
- [java-1.8.0-openjdk-headless.x86_64](https://yum.oracle.com/repo/OracleLinux/OL6/8/base/x86_64/getPackage/java-1.8.0-openjdk-headless-1.8.0.91-1.b14.el6.x86_64.rpm)

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


# Startup database
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
```console
SQL> @apex_rest_config.sql
Enter a password for the APEX_LISTENER user              [] 
Enter a password for the APEX_REST_PUBLIC_USER user              [] 
...set_appun.sql
...setting session environment
...create APEX_LISTENER and APEX_REST_PUBLIC_USER users

```

# Unlock một số user cần thiết
```SQL
/* Cần unlock một số user liên quan
   - APEX: APEX_PUBLIC_USER, APEX_LISTENER, APEX_REST_PUBLIC_USER
   - ORDS: ORDS_PUBLIC_USER
*/
SELECT USERNAME,ACCOUNT_STATUS  FROM DBA_USERS WHERE USERNAME LIKE 'APEX%';

ALTER USER APEX_LISTENER  ACCOUNT UNLOCK identified by "Passw0rd!2";
ALTER USER APEX_PUBLIC_USER ACCOUNT UNLOCK identified by "Passw0rd!2";
ALTER USER APEX_REST_PUBLIC_USER ACCOUNT UNLOCK identified by "Passw0rd!2";
--ALTER USER APEX_INSTANCE_ADMIN_USER ACCOUNT UNLOCK identified by "Passw0rd!2";
--ALTER USER APEX_200200 ACCOUNT UNLOCK identified by "Passw0rd!2";

SELECT USERNAME,ACCOUNT_STATUS  FROM DBA_USERS WHERE USERNAME LIKE 'ORDS%';

--ALTER USER ORDS_METADATA ACCOUNT UNLOCK identified by "Passw0rd!2";
ALTER USER ORDS_PUBLIC_USER ACCOUNT UNLOCK identified by "Passw0rd!2";
--ALTER USER ORDSYS ACCOUNT UNLOCK identified by "Passw0rd!2";
```

# Mở kết nối
11g
```SQL
DECLARE
   ACL_PATH   VARCHAR2 (4000);
BEGIN
   -- Look for the ACL currently assigned to '*' and give APEX_200200----------( Or APEX_200200)
   -- the "connect" privilege if APEX_200200 does not have the privilege yet.

   SELECT ACL
     INTO ACL_PATH
     FROM DBA_NETWORK_ACLS
    WHERE HOST = '*' AND LOWER_PORT IS NULL AND UPPER_PORT IS NULL;


   IF DBMS_NETWORK_ACL_ADMIN.CHECK_PRIVILEGE (ACL_PATH,
                                              'APEX_200200',
                                              'connect')
         IS NULL
   THEN
      DBMS_NETWORK_ACL_ADMIN.ADD_PRIVILEGE (ACL_PATH,
                                            'APEX_200200',
                                            TRUE,
                                            'connect');
   END IF;
EXCEPTION
   -- When no ACL has been assigned to '*'.

   WHEN NO_DATA_FOUND
   THEN
      DBMS_NETWORK_ACL_ADMIN.CREATE_ACL (
         'power_users.xml',
         'ACL that lets power users to connect to everywhere',
         'APEX_200200',
         TRUE,
         'connect');

      DBMS_NETWORK_ACL_ADMIN.ASSIGN_ACL ('power_users.xml', '*');
END;
/

COMMIT;

DECLARE
   ACL_PATH   VARCHAR2 (4000);
BEGIN
   -- Look for the ACL currently assigned to 'localhost' and give APEX_200200
   -- the "connect" privilege if APEX_200200 does not have the privilege yet.

   SELECT ACL
     INTO ACL_PATH
     FROM DBA_NETWORK_ACLS
    WHERE HOST = 'localhost' AND LOWER_PORT IS NULL AND UPPER_PORT IS NULL;

   IF DBMS_NETWORK_ACL_ADMIN.CHECK_PRIVILEGE (ACL_PATH,
                                              'APEX_200200',
                                              'connect')
         IS NULL
   THEN
      DBMS_NETWORK_ACL_ADMIN.ADD_PRIVILEGE (ACL_PATH,
                                            'APEX_200200',
                                            TRUE,
                                            'connect');
   END IF;
EXCEPTION
   -- When no ACL has been assigned to 'localhost'.

   WHEN NO_DATA_FOUND
   THEN
      DBMS_NETWORK_ACL_ADMIN.CREATE_ACL (
         'local-access-users.xml',
         'ACL that lets users to connect to localhost',
         'APEX_200200',
         TRUE,
         'connect');

      DBMS_NETWORK_ACL_ADMIN.ASSIGN_ACL ('local-access-users.xml',
                                         'localhost');
END;
/

COMMIT;
```

```SQL
--For Oracle Database version 12c or later run the below script:


BEGIN
DBMS_NETWORK_ACL_ADMIN.APPEND_HOST_ACE(
host => '*',
ace => xs$ace_type(privilege_list => xs$name_list('connect'),
principal_name => 'APEX_200200',
principal_type => xs_acl.ptype_db));
END;
/

BEGIN
DBMS_NETWORK_ACL_ADMIN.APPEND_HOST_ACE(
host => 'localhost',
ace => xs$ace_type(privilege_list => xs$name_list('connect'),
principal_name => 'APEX_200200',
principal_type => xs_acl.ptype_db));
END;
/

EXEC DBMS_XDB.sethttpport(0);

```

# Cài đặt ORDS

```console
[oracle@dbvnpay ords]$ java -jar ords.war install
This Oracle REST Data Services instance has not yet been configured.
Please complete the following prompts


Enter the location to store configuration data: /u01/ords/config
Enter the name of the database server [localhost]:
Enter the database listen port [1521]:
Enter 1 to specify the database service name, or 2 to specify the database SID [1]:2
Enter the database SID [xe]:VNPAY
Enter the database password for ORDS_PUBLIC_USER:
Confirm password:
Requires to login with administrator privileges to verify Oracle REST Data Services schema.

Enter the administrator username:sys
Enter the database password for SYS AS SYSDBA:
Confirm password:
Connecting to database user: SYS AS SYSDBA url: jdbc:oracle:thin:@localhost:1521:VNPAY

Retrieving information.
Enter 1 if you want to use PL/SQL Gateway or 2 to skip this step.
If using Oracle Application Express or migrating from mod_plsql then you must enter 1 [1]:
Enter the database password for APEX_PUBLIC_USER:
Confirm password:
Enter 1 to specify passwords for Application Express RESTful Services database users (APEX_LISTENER, APEX_REST_PUBLIC_USER) or 2 to skip this step [1]:
Enter the database password for APEX_LISTENER:
Confirm password:
Enter the database password for APEX_REST_PUBLIC_USER:
Confirm password:
Enter a number to select a feature to enable:
   [1] SQL Developer Web  (Enables all features)
   [2] REST Enabled SQL
   [3] Database API
   [4] REST Enabled SQL and Database API
   [5] None
Choose [1]:1
2020-12-29T02:01:28.382Z INFO        reloaded pools: []
Installing Oracle REST Data Services version 20.3.0.r3011819
... Log file written to /home/oracle/ords_install_core_2020-12-29_090128_00471.log
... Verified database prerequisites
... Created Oracle REST Data Services proxy user
... Created Oracle REST Data Services schema
... Granted privileges to Oracle REST Data Services
... Created Oracle REST Data Services database objects
... Log file written to /home/oracle/ords_install_datamodel_2020-12-29_090139_00061.log
... Log file written to /home/oracle/ords_install_apex_2020-12-29_090139_00748.log
Completed installation for Oracle REST Data Services version 20.3.0.r3011819. Elapsed time: 00:00:12.44 

Enter 1 if you wish to start in standalone mode or 2 to exit [1]:1
Enter the APEX static resources location:/u01/ords/images
Enter 1 if using HTTP or 2 if using HTTPS [1]:1

```

# [Start and Stop ORDS](https://oracle-base.com/articles/misc/oracle-rest-data-services-ords-standalone-mode)

```console
[oracle@dbvnpay ~]$ pwd
/home/oracle
[oracle@dbvnpay ~]$ mkdir -p ~/scripts/logs
[oracle@dbvnpay ~]$ 
[oracle@dbvnpay ~]$ cd scripts/
[oracle@dbvnpay ~]$ vi start_ords.sh

#!/bin/bash
export PATH=/usr/sbin:/usr/local/bin:/usr/bin:/usr/local/sbin:$PATH
export JAVA_HOME=/usr
LOGFILE=/home/oracle/scripts/logs/ords-`date +"%Y""%m""%d"`.log
cd /u01/ords
export JAVA_OPTIONS="-Dorg.eclipse.jetty.server.Request.maxFormContentSize=3000000"
nohup $JAVA_HOME/bin/java ${JAVA_OPTIONS} -jar ords.war standalone >> $LOGFILE 2>&1 &
echo "View log file with : tail -f $LOGFILE"

[oracle@dbvnpay ~]$ vi stop_ords.sh

#!/bin/bash
export PATH=/usr/sbin:/usr/local/bin:/usr/bin:/usr/local/sbin:$PATH
kill `ps -ef | grep ords.war | awk '{print $2}'`

[oracle@dbvnpay ~]$ chmod u+x ~/scripts/*.sh

```

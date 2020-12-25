
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
startup database

truy cap vao sqlplus / as sysdba

@apexins.sql sysaux sysaux temp /i/



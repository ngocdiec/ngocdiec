Các loại file trong Oracle:
PFILEs/SPFILEs: Khi một Oracle Instance được khởi tạo, các đặc tính của nó cũng được thiết lập bởi các tham số cấu hình được lưu trữ trong PFILEs/SPFILEs (SPFILEs (Server Parameter File) chỉ có từ phiên bản 9 trở lên, còn các phiên bản trước đó sử dụng PFILEs)
[controlfile](https://docs.oracle.com/en/database/oracle/oracle-database/19/admin/managing-control-files.html#GUID-98A05D29-DD80-4D87-9615-76CBCF8FE694): Every Oracle Database has a control file, which is a small binary file that records the physical structure of the database.
The control file includes:
- The database name
- Name and locations of associated files and redo log file
- The timestamp of the database creation
- The current lof sequence number
- Checkpoint infomation

pwfile

# Startup and Shutdown modes in Oracle
## Startup modes:
### STARTUP NOMOUNT
- Oracle open and read spfile/pfile
- Instance gets created (SGA + BP)
- We can create a database
- We can recreate controlfile
- Base on the values from spfile/pfile oracle will allocate the SGA in the RAM and start the background process

### STARTUP MOUNT (Maintance state)
- Oracle open and read control file
- We can perform recovery
- We can enable ALM (Archive log mode)
- We can enable FDBD (Flashback database)

Các loại file trong Oracle:
1. PFILEs/SPFILEs: Khi một Oracle Instance được khởi tạo, các đặc tính của nó cũng được thiết lập bởi các tham số cấu hình được lưu trữ trong PFILEs/SPFILEs (SPFILEs (Server Parameter File) chỉ có từ phiên bản 9 trở lên, còn các phiên bản trước đó sử dụng PFILEs)
2. [CONTROLFILE](https://docs.oracle.com/en/database/oracle/oracle-database/19/admin/managing-control-files.html#GUID-98A05D29-DD80-4D87-9615-76CBCF8FE694): Every Oracle Database has a control file, which is a small binary file that records the physical structure of the database. The control file includes:
   - The database name
   - Name and locations of associated files and redo log file
   - The timestamp of the database creation
   - The current lof sequence number
   - Checkpoint infomation

pwfile

# Startup and Shutdown modes in Oracle
![Startup and Shutdown modes in Oracle](https://www.ktexperts.com/wp-content/uploads/2019/01/1-2.png)
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

### OPEN
- Databse completely open, where end users connect and perform all transactions
- While moving from mount to open state Oracler perform "Sanity checking"
- According to the controlfile infomation, Oracle check for physical existence of files and chck th synchronization SCN#, which is know as "Sanity checking"

## Shutdown modes
### SHUTDOWN NORMAL
- New connection are not allowed
- Connected user can perform ongoing transaction
- Idle sesstion will not disconected
- When connected user's logout manual then the database gets shutdown
- It is also graceful shutdown. So it doesn't require ICR in next startup
- A common SCN number will be updated to controlfiles and datafile before the database shutdown 
### SHUTDOWN TRANSACTION
- New connection are not allowed
- Connected user can perform ongoing transaction
- *Idle session will be disconected*
- *The database gets shutdown once ongoing transactions gets completed (COMMIT/ROLLBACK)*
- Hence, it is also graceful shutdown. So it doesn't require ICR in next startup
### SHUTDOWN IMMEDIATE
- New connection are not allowed
- *Connected user can't perform ongoing transaction*
- Idle session will be disconected
- *Oracle perform ROLLBACKs the ongoing transaction (uncommited) and database gets shutdown*
- A common SCN number will be updated to controlfiles and datafiles before the database shutdown
- Hence, it is also graceful shutdown. So it doesn't require ICR in next startup
### SHUTDOWN ABORT
- New connections are not allowed
- Connected user can't perform ongoing transaction
- Idle session will be disconnected
- *Database gets shutdown abruptly (No COMMIT/ No ROLLBACK)*
- *Hence. it is abruptly shutdown. So its require ICR in next startup*

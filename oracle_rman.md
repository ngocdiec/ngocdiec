# RMAN (Recovery Manager)
RMAN là một Oracle Database client thực hiện các nhiệm vụ sao lưu và phục hồi trên cơ sở dữ liệu và tự động hóa việc quản trị các chiến lược sao lưu của bạn. Nó giúp đơn giản hóa đang kể việc sao lưu, khôi phục.

**Tối thiểu RMAN phải bao gồm các thành phần sau:**
- target database
> Là database mà RMAN thực hiện các hoạt động backup and recovery
- RMAN client
> Là tệp thực thi, được tự động cài đặt cùng với database trong cùng thư mục (thường nằm trong: $ORACLE_HOME/bin)

**Một số thành phần tùy chọn**
- fast recovery area
> Là một vị trí ổ đĩa trong đó database có thể lưu trữ và quản lý các tệp liên quan đến backup and recovery. Được cài đặt thông qua 2 tham số khởi tạo: DB_RECOVERY_FILE_DEST và DB_RECOVERY_FILE_DEST_SIZE
- media manager
> Một ứng dụng cần thiết để RMAN có thể tương tác với các thiết bị phương tiện tuần tự ví dụ như SBT (System Backup to Tape)
- recovery catalog
> Một schema riêng biệt được sử dụng để ghi lại hoạt động của RMAN

## Starting RMAN
```console
RMAN TARGET /
```

## Showing the Default RMAN Configuration
```console
RMAN> SHOW ALL;
```

## Backing Up a Database
Mặc định RMAN sẽ backup vào FRA, và sinh file name với tên duy nhất nếu như không chỉ định FORMAT cho file name
FORMAT
|Format|Meaning|
|-|-|
|%U|generates a unique name|
|%d|DB_NAME|
|%t|backup set time stamp|
|%s|backup set number|
|%p|backup piece number|

**Kiểm tra Database log mode**
```SQL
SQL> ARCHIVE LOG LIST;
```
### In ARCHIVELOG Mode
Nếu database đang chạy ở `ARCHIVELOG` mode, thì ta có thể backup database khi nó đang được `OPEN` (hot backup). Yêu cầu cần có ARCHIVELOG (redo log files) để có thể recover.

To back up the database and archived redo logs while the database is open:
1. Start RMAN and connect to a target database.
2. Run the `BACKUP DATABASE` command.

   For example, enter the following command at the RMAN prompt to back up the database and all archived redo log files to the default backup device:
   ```console
   RMAN> BACKUP DATABASE PLUS ARCHIVELOG;
   ```

### In NOARCHIVELOG Mode
Nếu database đang chạy ở `NOARCHIVELOG ` mode, thì ta chỉ có thể backup được khi database chưa được `OPEN`, tức là ở `STARTUP MOUNT` state

To make a consistent database backup:
1. Start RMAN and connect to a target database.
2. Shut down the database consistently and then mount it.

   For example, enter the following commands to guarantee that the database is in a consistent state for a backup:
   ```console
   RMAN> SHUTDOWN IMMEDIATE;
   RMAN> STARTUP FORCE DBA;
   RMAN> SHUTDOWN IMMEDIATE;
   RMAN> STARTUP MOUNT;
   ```
3. Run the BACKUP DATABASE command.

   For example, enter the following command at the RMAN prompt to back up the database to the default backup device:
   ```console
   RMAN> BACKUP DATABASE;
   ```
   The following variation of the command creates image copy backups of all data files in the database:
   ```console
   RMAN> BACKUP AS COPY DATABASE;
   ```
4. Open the database and resume normal operations.

   The following command opens the database:
   ```console
   RMAN> ALTER DATABASE OPEN;
   ```

### Making Incremental Backups

```console
run{
  # archive RMAN catalog entries
  crosscheck archivelog all;
  crosscheck backup;
  #
  delete expired backup;
  #
  backup incremental level 0 database format '/backup/rman/data/%T_data_%U';
  # Backup Archived Logs
  backup archivelog all format '/backup/rman/data/%T_arc_%U' delete input;
  # Control file backup
  backup current controlfile format '/backup/rman/data/%T_ctl_%U';
  # Spfile backup
  backup spfile format '/backup/rman/data/%T_spfile_%U';
  crosscheck backup;
  #
  delete obsolete;
}
```
crosscheck: sẽ kiểm tra RMAN repository với các file trên ổ đĩa để xác định expired và obsolete



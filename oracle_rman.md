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

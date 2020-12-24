# MongoDB Comunity Incremental Backup
Dựa vào local.oplog.rs để thực hiện Incremental Backup
- local.oplog.rs là gì
- các field trong colelction ý nghĩa như thế nào
- cơ chế hoạt động của collecion này


MongoDB Incremental Backup

dump sự thay đổi trong local.oplog_rs dựa vào field ts
restore lại với tham số -oplogReplay


https://www.percona.com/blog/2016/09/20/mongodb-point-in-time-backups-made-easy/

forked
https://github.com/m-masataka/mongo-tools/tree/incremental_backup

https://navyuginfo.com/incremental-backup-for-mongodb/


conn_args="--host 10.22.7.222 --port 27017 -u admin -p vnpadmin7222test --authenticationDatabase admin"

mongodump ${conn_args} -d local -c oplog.rs --query '{"ts":{"$timestamp":{"t":1604476288,"i":3456}}}' --out /backup/mongomove/




mongodump ${conn_args} -d local -c oplog.rs --query '{"ts":{"$gte":{"$timestamp":{"t":1604476288,"i":3456}}}}' --out /backup/mongomove/

vnpsys730test

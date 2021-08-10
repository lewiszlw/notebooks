登录数据库：mysql -u {user} -p{password} （-p 和 password 之间无空格）

查看所有数据库：show databases;

选择数据库：use {database};

查看当前数据库所有表：show tables;

查看表结构：desc {table};

查询数据库状态：
- show status like '%Max_used_connections%';
- show status like '%Threads_connected%'; #当前连接数
- show status like '%table_lock%'; #表锁定
- show status like 'innodb_row_lock%'; #行锁定
- show status like '%qcache%'; #查询缓存情况
- show variables like "%query_cache%";
- show variables like "%binlog%";
- show status like 'Aborted_clients'; #由于客户没有正确关闭连接已经死掉，已经放弃的连接数量
- show variables like '%max_connections%'; #查看最大连接数量
- show variables like '%timeout%'; #查看超时时间
- show variables like 'log_%'; #查看日志是否启动


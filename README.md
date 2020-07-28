# shardingjdbc
# mysql主从同步配置
# 启动2个容器
docker run -d --name master -v /e/mysql.cnf:/etc/mysql/mysql.cnf -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 mysql:5.7

docker run -d --name master -v /f/mysql.cnf:/etc/mysql/mysql.cnf -e MYSQL_ROOT_PASSWORD=root -p 3307:3306 mysql:5.7

# 主库的配置文件mysql.cnf
[mysqld]

server-id=1
log-bin=log
binlog-ignore-db=mysql
binlog-ignore-db=information_schema
binlog-ignore-db=performance_schema
binlog-ignore-db=sys

# 从库的配置文件mysql.cnf

[mysqld]
server-id=2
log-bin=mysql-bin
replicate-ignore-db=mysql
replicate-ignore-db=information_schema
replicate-ignore-db=performance_schema
replicate-ignore-db=sys
log-slave-updates
slave-skip-errors=all
slave-net-timeout=60

stop slave;
change master to master_host='192.168.1.126',
master_user='slave',
master_password='root',
master_log_file='mysql-bin.000013',
master_log_pos=1175; 
start slave;

master_host 主库ip  
master_user 上面新建的用户
master_password 上面新建用户的密码
master_log_file  主库命令行中的file值
master_log_pos 主库命令行中的position值

最后重启服务进行测试

# 下载github包
https://d.serctl.com/

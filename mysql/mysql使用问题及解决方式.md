### mysql设置表名大小写不敏感
#### mysql8
mysql8设置大小写不敏感，必须在安装的时候才能启用，因为新的’Data Dictionary’需要在设置不能更改状态下构建一次。
##### centos示例
```shell
yum remove mysql mysql-server
yum install mysql mysql-server
vi /etc/my.cnf
lower_case_table_names=1
## 必须在第一次启动mysql server前，添加lower_case_table_names=1到my.cnf.
systemctl start mysqld
```

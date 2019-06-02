## Docker for Mysql升级
> 本文基于Mysql5.6-->mysql8.0，介绍数据迁移的过程和工具的简单实用，具体原理并未涉及。

#### 数据导出备份
>命令加粗的地方需要根据实际填写
OLDER_DOCKER_MYSQL_CONTAINER_NAME:要导出的mysql容器名称  
EXPORT_DATABASE_NAME：要导出的数据库名称，多个数据库用空格分割  
LOCAL_EXPORT_PATH：将导出文件保存到本地文件系统的地址  


`docker exec **OLDER_DOCKER_MYSQL_CONTAINER_NAME** sh -c 'exec mysqldump -uroot -p"$MYSQL_ROOT_PASSWORD" --databases **EXPORT_DATABASE_NAME** '  > **LOCAL_EXPORT_PATH**`

##### 案例：
```shell
docker exec mysql sh -c 'exec mysqldump -uroot -p"$MYSQL_ROOT_PASSWORD" --databases bee-order bee-product cms flower interesting leetcode niuke niukewang qms_local ry spider '  > /Users/tobbyquinn/docker/mysql8/all-databases.sql
```  
##### 涉及到的坑
mysql5.X 和 mysql8.0 的系统表结构发生了一些变化，所以在使用 mysqldump 命令时，不能使用`--all-databses`**(全部数据库导出参数)**,需要先排除mysql等系统数据库，mysqldump 命令并未提供 exclude 选项**(本文写作时未提供)**,需要大家一个个写，当然也可以通过脚本代替该笨重方法


#### 安装新MYSQL版本容器,并自动恢复数据库数据
> 大多参数属于docker run，不会的可以参考[官方文档](https://docs.docker.com/engine/reference/run/)  
着重讲下初始化导出数据库参数 -v：将本地导出文件的路径映射到/docker-entrypoint-initdb.d容器路径即可,在安装完数据库后，会自动加载导出数据数据。  
**注意:导出文件的格式必须为.sh, .sql and .sql.gz这几种才会自动加载!!!**  

docker run -it --name **NEW_DOCKER_MYSQL_CONTAINER_NAME** -p 3306:3306 -v **LOCAL_EXPORT_PATH**:/docker-entrypoint-initdb.d -e MYSQL_ROOT_PASSWORD=root -d **MSYQL_DOCKER_VERSION** --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
##### 案例
```shell
docker run -it --name mysql8 -p 3306:3306 -v /Users/tobbyquinn/docker/mysql8/:/docker-entrypoint-initdb.d -e MYSQL_ROOT_PASSWORD=root -d mysql:8.0 --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```
#### 参考链接
[docker_mysql](https://hub.docker.com/_/mysql)  
[mysqldump docs](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html)

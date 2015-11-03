# Docker For MySQL

### 版本

基于ubuntu:trusty构建

### 说明
- 构造镜像
`docker build -t="$USER/mysql" .`
- 启动容器
`docker run --name mysql -d $USER/mysql`
- 支持的环境变量如下：
 * DB_NAME：创建数据库，支持新建多个数据库`DB_NAME=dbname1,dbname2`
 * DB_USER：设置针对上述数据库的用户名
 * DB_PASS：设置用户密码
 > 例：
 > ```
 > docker run --name mysql -d \
 > -e 'DB_USER=dbuser' \
 > -e 'DB_PASS=dbpass' \
 > -e 'DB_NAME=dbname' \
 > $USER/mysql
 > ```
 > 上面的命令将会创建一dbuser用户，并新建一个dbname数据库，dbuser拥有对dbname的全部&远程权限
 > >**Note**
 >
 > >1.指定DB_USER必须指定DB_NAME
 > >2.如果DB_PASS未指定，默认为空
 > >3.可以仅指定DB_NAME

  *  DB_REMOTE_ROOT_NAME：指定具有root权限的远程用户
  *  DB_REMOTE_ROOT_PASS： 指定相应的密码
  *  DB_REMOTE_ROOT_HOST： 指定远程访问的地址限制，默认为“172.17.42.1“

- 挂载目录到容器中
   需要将外部目录挂载到容器中的/var/lib/mysql中，以保证/var/lib/mysql中的数据不会因容器停止而丢失，实现数据库文件持久化
   >例：
   >
   >```
   >docker run --name mysql -d \
   >-v /opt/mysql/data:/var/lib/mysql \
   >$USER/mysql
   >```
- 默认该容器只允许通过link的方式从别的容器内部访问数据库服务，若要从容器外部连接，需映射3306端口
  `docker run --name mysql -d -p 3306:3306 $USER/mysql`
### 使用Docker构建Linux+Apache+Mysql+PHP开发环境

-----------------------------------------

1. 安装docker-compose组件
   `sudo -E pip install -U docker-compose`

2. 修改docker-compose.yml文件中的配置

	```
php-apache:
 build: ./php-apache/
 ports:
 - "10080:80"   // 将容器内的80端口映射到本机的10080端口
 volumes:
 - ~/app:/app   // 改为你想要挂载的php项目目录
 links:
 - mysql
mysql:
 build: ./mysql/   // 可以更改为其他mysql容器镜像， 例如：image: mysql:latest
 environment:
 - DB_REMOTE_ROOT_NAME=admin      // 管理员用户名
 - DB_REMOTE_ROOT_PASS=admin      // 管理员密码
 - DB_REMOTE_ROOT_HOST=%          // 管理员访问控制
 - DB_USER=dbuser                 // 项目所使用的数据库用户
 - DB_PASS=dbpass                 // 项目所使用的数据库密码
 - DB_NAME=dbname_for_php         // 项目所使用的数据库名
	```

3. 进入docker-compose.yml所在目录运行
`docker-compose -p <your-prefix> up -d`
将会启动并连接名为`<your prefix>_php-apache`和`<your prefix>_mysql`的两个容器。

4. 将项目放到本机的～/app目录 访问`localhost:10080/<project-name>`
   >在项目中使用`$conn=mysql_connect("mysql", "dbuser", "dbpass");`连接数据库，此处连接地址可以直接写成`mysql`,与`docker-compose.yml`文件中`links`映射的mysql容器名有关。
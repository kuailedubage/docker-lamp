# Docker For PHP and Apache

### 版本

基于ubuntu:trusty构建

### 说明
- 构造镜像
`docker build -t="$USER/apache-php" .`

- 启动容器
`docker run -d -p 80:80 $USER/apache-php`

- 支持的环境变量如下：

  ALLOW_OVERRIDE:开启.htaccess
 `docker run -d -p 80:80 -e ALLOW_OVERRIDE=true $USER/apache-php`
- 通过挂载目录到容器中的/app使用自定义的网页文件

  `docker run -d -p 80:80 -v /your/app/path:/app $USER/apache-php`
  访问本地localhost页面查看效果
- 使用\--link参数可以连接其他容器
  ```
  docker run --name mysql -d \
  -e 'DB_USER=dbuser' \
  -e 'DB_PASS=dbpass' \
  -e 'DB_NAME=dbname_for_php' \
  $USER/mysql
  docker run -d \
  -p 80:80 \
  -v /your/app/path:/app \
  --link mysql:db \
  $USER/apache-php
  ```
  在/your/app/path中新建index.php文件
  ```php
  <?php
  $con = mysql_connect("db","dbuser","dbpass");
  if (!$con)
    {
    die('Could not connect: ' . mysql_error());
    }
  $db_selected = mysql_select_db("dbname_for_php", $con);
  if (!$db_selected)
    {
    die ("Can\'t use dbname_for_php : " . mysql_error());
    }
  mysql_close($con);
  // some code
  ?>
  ```
# Ubuntu 14.04，Trusty
FROM ubuntu:trusty
MAINTAINER kuailedubage <kuailedubage521@gmail.com>

# 设置代理
#RUN echo "deb http://tw.archive.ubuntu.com/ubuntu trusty main universe\n" \
#     > /etc/apt/sources.list \
# && echo "deb http://tw.archive.ubuntu.com/ubuntu trusty-updates main universe\n" \
#     >> /etc/apt/sources.list \
# && touch /etc/apt/apt.conf \
# && echo "Acquire::http::proxy \"http://ip:port/\";" > /etc/apt/apt.conf \
# && echo "Acquire::https::proxy \"https://ip:port/\";" >> /etc/apt/apt.conf \
# && echo "Acquire::ftp::proxy \"ftp://ip:port/\";" >> /etc/apt/apt.conf \
# && echo "Acquire::socks::proxy \"socks://ip:port/\";" >> /etc/apt/apt.conf

# 设置环境变量
ENV MYSQL_USER=mysql \
    MYSQL_DATA_DIR=/var/lib/mysql \
    MYSQL_RUN_DIR=/run/mysqld \
    MYSQL_LOG_DIR=/var/log/mysql

# APT 自动安装 MySQL 相关的依赖包，如需其他依赖包在此添加
# 使用-qy参数去掉不必要的日志和提问，使用--no-install-recommends参数来避免安装非必须的文件
# 仅为apt-get install命令设置环境变量DEBIAN_FRONTEND，取消交互
RUN apt-get update -qq \
 && DEBIAN_FRONTEND=noninteractive apt-get -qy --no-install-recommends install \
 mysql-server \
# supervisor \
 && rm -rf ${MYSQL_DATA_DIR} \
 && rm -rf /var/lib/apt/lists/*

# 使用supervisord管理服务
# ADD supervisord-mysqld.conf /etc/supervisor/conf.d/supervisord-mysqld.conf

ADD entrypoint.sh /entrypoint.sh
RUN chmod 755 /entrypoint.sh

VOLUME ["${MYSQL_DATA_DIR}", "${MYSQL_RUN_DIR}"]
EXPOSE 3306
ENTRYPOINT ["/entrypoint.sh"]



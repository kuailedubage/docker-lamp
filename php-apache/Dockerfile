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

# APT 自动安装 PHP 相关的依赖包，如需其他依赖包在此添加
# 使用-qy参数去掉不必要的日志和提问，使用--no-install-recommends参数来避免安装非必须的文件
# 仅为apt-get install命令设置环境变量DEBIAN_FRONTEND，取消交互
RUN apt-get update -qq \
    && DEBIAN_FRONTEND=noninteractive apt-get -qy --no-install-recommends install \
        curl \
        wget \
        apache2 \
        libapache2-mod-php5 \
        php5-mysql \
        php5-mcrypt \
        php5-gd \
        php5-curl \
        php-pear \
        php-apc \
        supervisor \

    # 删除不必要的软件和Apt缓存包列表以减少镜像大小
    && apt-get clean \
    && apt-get autoclean \
    && apt-get autoremove \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
#    && export http_proxy=http://ip:port/ \
#    && export ftp_proxy=http://ip:port/ \
#    && export https_proxy=http://ip:port/ \
    # 安装 Composer，PHP 用来管理依赖关系的工具，Laravel Symfony等框架会依赖它
    && curl -sSk https://getcomposer.org/installer \
        | php -- --install-dir=/usr/local/bin --filename=composer

RUN /usr/sbin/php5enmod mcrypt
# 给 Apache 2 设置一个默认服务名，避免启动时警告
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf \
    # 调整 PHP 处理 Request 里变量提交值的顺序，解析顺序从左到右，后解析新值覆盖旧值
    # 默认设定为 EGPCS（ENV/GET/POST/COOKIE/SERVER）
    && sed -i 's/variables_order.*/variables_order = "EGPCS"/g' /etc/php5/apache2/php.ini

# 开启.htaccess
ENV ALLOW_OVERRIDE **False**

# 配置默认放置 App 的目录
RUN mkdir -p /app && rm -rf /var/www/html && ln -s /app /var/www/html

# 使用supervisord管理服务
ADD supervisord-apache2.conf /etc/supervisor/conf.d/supervisord-apache2.conf
ADD start-apache2.sh /start-apache2.sh
RUN chmod 755 /start-apache2.sh

ADD entrypoint.sh /entrypoint.sh
RUN chmod 755 /entrypoint.sh

EXPOSE 80
WORKDIR /app
ENTRYPOINT ["/entrypoint.sh"]

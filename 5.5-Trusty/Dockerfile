FROM ubuntu:14.04
MAINTAINER clickap

ADD ./foreground.sh /etc/apache2/foreground.sh

VOLUME ["/var/www/moodledata"]

EXPOSE 80

ENV TZ=Asia/Taipei

RUN apt-get update && \
    apt-get install -y  curl wget git unzip apache2 php5 libapache2-mod-php5 \
        php5-mysql php5-curl php5-xmlrpc php5-gd php5-intl php5-ldap && \
    rm -rf /var/lib/apt/lists/* && \
    echo "AcceptPathInfo On" >> /etc/apache2/httpd.conf && \
    sed -i "s/;date.timezone =/date.timezone = \"Asia\/Taipei\"/g" /etc/php5/apache2/php.ini && \
    sed -i "s/;date.timezone =/date.timezone = \"Asia\/Taipei\"/g" /etc/php5/cli/php.ini && \
    sed -i 's/post_max_size = 8M/post_max_size = 1024M/g' /etc/php5/apache2/php.ini && \
    sed -i 's/post_max_size = 8M/post_max_size = 1024M/g' /etc/php5/cli/php.ini && \
    sed -i 's/upload_max_filesize = 2M/upload_max_filesize = 1024M/g' /etc/php5/apache2/php.ini && \
    sed -i 's/upload_max_filesize = 2M/upload_max_filesize = 1024M/g' /etc/php5/cli/php.ini && \
    sed -i 's/max_execution_time = 30/max_execution_time = 600/g' /etc/php5/apache2/php.ini && \
    sed -i 's/max_execution_time = 30/max_execution_time = 600/g' /etc/php5/cli/php.ini && \
    sed -i 's/max_input_time = 60/max_input_time = 600/g' /etc/php5/apache2/php.ini && \
    sed -i 's/max_input_time = 60/max_input_time = 600/g' /etc/php5/cli/php.ini && \
    sed -i 's/memory_limit = 128M/memory_limit = 1024M/g' /etc/php5/apache2/php.ini && \
    sed -i 's/memory_limit = 128M/memory_limit = 1024M/g' /etc/php5/cli/php.ini && \
    sed -i 's/Options Indexes FollowSymLinks/Options -FollowSymLinks/g' /etc/apache2/apache2.conf && \
    git clone https://github.com/susanclickap/cwb.git /var/www/html/cwb && \
    chown -R www-data:www-data /var/www/html/cwb && \
    chmod 744 /var/www/html/cwb/lang -R


RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone


ADD http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.bz2 /tmp/

RUN tar xvjfC /tmp/ioncube_loaders_lin_x86-64.tar.bz2 /tmp/ \
    && rm /tmp/ioncube_loaders_lin_x86-64.tar.bz2 \
    && mkdir -p /usr/local/ioncube \
    && cp /tmp/ioncube/ioncube_loader_* /usr/local/ioncube \
    && rm -rf /tmp/ioncube
COPY 00-ioncube.ini /etc/php5/apache2/conf.d/00-ioncube.ini
COPY 00-ioncube.ini /etc/php5/cli/conf.d/00-ioncube.ini


CMD ["/etc/apache2/foreground.sh"]

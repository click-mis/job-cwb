FROM moodletw/httpd:5.6-centos7
MAINTAINER Click-AP

ENV Customer=cwb
ENV TZ=Asia/Taipei

USER root

RUN yum update -y \
 && yum install -y php-opcache php-ldap \
 && yum clean all

RUN sed -i \
 -e "s|;date.timezone =|date.timezone = 'Asia/Taipei'|g" \
 -e 's/post_max_size = 8M/post_max_size = 1024M/g' \
 -e 's/upload_max_filesize = 2M/upload_max_filesize = 1024M/g' \
 -e 's/max_execution_time = 30/max_execution_time = 600/g' \
 -e 's/max_input_time = 60/max_input_time = 600/g' \
 -e 's/memory_limit = 128M/memory_limit = 1024M/g' /etc/php.ini

VOLUME ["/var/moodledata"]

ADD trunk /var/www/html

# Simple startup script to avoid some issues observed with container restart
ADD entrypoint /usr/local/entrypoint
RUN chmod -v +x /usr/local/entrypoint \
 && mkdir -p /var/moodledata \
 && chown -R docker:apache /var/www/html/ \
 && chown -R docker:apache /var/moodledata/ \
 && ln -sf /dev/stdout /var/log/httpd/access_log \
 && ln -sf /dev/stderr /var/log/httpd/error_log


USER docker

EXPOSE 80
WORKDIR /var/www/html
ENTRYPOINT ["/tini", "--", "/usr/local/entrypoint"]

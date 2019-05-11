FROM centos

LABEL maintainer="gwf <943242261@qq.com>"
LABEL description="基于 centos 的 php 镜像"
LABEL version="7.2.18"

WORKDIR /usr/local/src

RUN yum -y install wget gcc-c++ make file bison libxml2-devel openssl-devel bzip2-devel curl-devel libpng-devel \
    && yum clean all \
    && wget https://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.16.tar.gz \
    && tar -xzvf libiconv-1.16.tar.gz \
    && cd libiconv-1.16 \
    && ./configure --prefix=/usr/local/libiconv \
    && make && make install

ENV PHP_VERSION 7.2.18
ENV PHP_INI_DIR /etc/php

# RUN wget https://www.php.net/distributions/php-$PHP_VERSION.tar.bz2
ADD php-$PHP_VERSION.tar.bz2 /usr/local/src/
WORKDIR php-$PHP_VERSION
RUN ./configure --with-config-file-path=$PHP_INI_DIR --with-config-file-scan-dir=$PHP_INI_DIR/conf.d --enable-fpm --enable-bcmath --enable-mbstring --enable-soap --with-openssl --with-zlib --with-bz2 --with-curl --with-gd --with-gettext --with-iconv=/usr/local/libiconv --with-mysqli --without-pear --disable-fileinfo \
    && make && make install \
    && mkdir -p $PHP_INI_DIR/conf.d \
    && cp php.ini-* $PHP_INI_DIR/ \
    && cd $PHP_INI_DIR \
    && cp php.ini-production php.ini \
    && cd /usr/local/etc \
    && sed 's!=NONE/!=!g' php-fpm.conf.default | tee php-fpm.conf \
    && cp php-fpm.d/www.conf.default php-fpm.d/www.conf \
    && echo -e '[global]\ndaemonize = no' | tee php-fpm.d/zz-docker.conf \
    && rm -rf /usr/local/src

COPY docker-php-entrypoint /usr/local/bin/
ENTRYPOINT ["docker-php-entrypoint"]

STOPSIGNAL SIGQUIT

EXPOSE 9000

CMD ["php-fpm"]

FROM centos:7.4.1708

LABEL maintainer="gwf <943242261@qq.com>"
LABEL description="基于 centos 的 nginx 镜像"
LABEL version="1.15.12"

RUN yum -y install wget gcc-c++ pcre pcre-devel zlib zlib-devel openssl openssl-devel \
    && yum clean all

WORKDIR /usr/local/src

ENV NGINX_VERSION 1.15.12
ENV NGINX_EXTRA_CONFIGURE_ARGS --sbin-path=/usr/sbin/nginx --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid

RUN wget http://nginx.org/download/nginx-$NGINX_VERSION.tar.gz \
    && tar -xzvf nginx-$NGINX_VERSION.tar.gz \
    && cd nginx-$NGINX_VERSION \
    && sed -i 's/Server:.*CRLF/Server: guiwenfeng.com" CRLF/g' src/http/ngx_http_header_filter_module.c \
    && ./configure $NGINX_EXTRA_CONFIGURE_ARGS --with-http_ssl_module \
    && make && make install \
    && rm -rf /usr/local/src \
    && useradd nginx -s /sbin/nologin -M

WORKDIR /var/www

EXPOSE 80

STOPSIGNAL SIGTERM

CMD ["nginx", "-g", "daemon off;"]

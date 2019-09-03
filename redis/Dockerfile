FROM centos:7.4.1708

LABEL maintainer="gwf <943242261@qq.com>"
LABEL description="基于 centos 的 redis 镜像"
LABEL version="4.0.14"

RUN yum -y install wget gcc-c++ make \
    && yum clean all

WORKDIR /usr/local/src

ENV REDIS_VERSION 4.0.14

RUN wget http://download.redis.io/releases/redis-$REDIS_VERSION.tar.gz \
    && tar -xzvf redis-$REDIS_VERSION.tar.gz \
    && cd redis-$REDIS_VERSION \
    && make \
    && mkdir /etc/redis \
    && cp redis.conf sentinel.conf /etc/redis/ \
    && sed -i 's/logfile.*/logfile "\/var\/log\/redis.log"/g' /etc/redis/redis.conf \
    && cd src \
    && cp redis-benchmark redis-check-aof redis-check-rdb redis-cli redis-sentinel redis-server /usr/local/bin/ \
    && rm -rf /usr/local/src

COPY docker-redis-entrypoint.sh /usr/local/bin/
ENTRYPOINT ["docker-redis-entrypoint.sh"]

WORKDIR /usr/local/bin

EXPOSE 6379

CMD ["redis-server", "/etc/redis/redis.conf"]

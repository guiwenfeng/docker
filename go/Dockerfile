FROM centos:7.4.1708

LABEL maintainer="gwf <943242261@qq.com>"
LABEL description="基于 centos 的 go 镜像"
LABEL version="1.12.9"

WORKDIR /usr/local/src

ENV GOLANG_VERSION 1.12.9
ENV GOROOT /usr/local/go
ENV GOPATH /var/go
ENV PATH $GOPATH/bin:$GOROOT/bin:$PATH

RUN yum -y install wget \
    && yum clean all \
    && wget https://dl.google.com/go/go${GOLANG_VERSION}.linux-amd64.tar.gz \
    && tar -C /usr/local -xzvf go${GOLANG_VERSION}.linux-amd64.tar.gz \
    && rm -rf /usr/local/src \
    && mkdir $GOPATH

WORKDIR $GOPATH

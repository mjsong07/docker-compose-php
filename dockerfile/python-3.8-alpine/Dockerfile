# 初始容器镜像
#
FROM python:3.8-alpine3.14
MAINTAINER vevor "my@vevor.com"

COPY requirements-base.txt /var/run/requirements.txt
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \
	&& apk update \
    && apk upgrade \
    && apk add libzip-dev \
	&& apk add autoconf \
	&& apk add gcc g++ make libffi-dev openssl-dev \
	&& apk add supervisor \
	&& apk add bash \
	&& mkdir -p /var/www/html/ \
	&& mkdir -p /etc/supervisor/logs \
    && touch /etc/supervisor/logs/supervisord.log \
    && chmod 777 /etc/supervisor/logs/supervisord.log \
	&& pip install gunicorn \
	&& pip install -r /var/run/requirements.txt

WORKDIR /var/www/html/

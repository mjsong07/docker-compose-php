FROM redis:3.2.4

LABEL maintainer="dev@vevor.com"

RUN apt-get update \
    && apt-get install -y iputils-ping \
    && apt-get install -y vim \
    && apt-get install -y procps \
    && apt-get install -y cron \
    && mkdir -p /usr/local/var/db/redis \
    && chmod -R 777 /usr/local/var/db/redis

RUN mkdir -p /usr/local/etc/redis/

#CMD [ "redis-server", "/usr/local/etc/redis/redis.conf" ]

#EXPOSE 8080


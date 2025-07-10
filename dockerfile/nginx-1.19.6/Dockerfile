FROM nginx:1.19.6

LABEL maintainer="dev@my.com"

RUN apt-get update \
    && apt-get install -y iputils-ping \
    && apt-get install -y vim \
    && apt-get install -y procps \
    && apt-get install -y cron \
    && mkdir -p /run/php80 \
    && echo "" >>/run/php80/php80-fpm.sock \
    && chmod 777 /run/php80/php80-fpm.sock

EXPOSE 80
EXPOSE 443

#CMD ["nginx", "-v"]


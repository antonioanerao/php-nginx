FROM nginx:alpine

VOLUME [ "/laravel" ]
WORKDIR /laravel

RUN apk update && apk upgrade

ADD nginx/default.conf /etc/nginx/conf.d

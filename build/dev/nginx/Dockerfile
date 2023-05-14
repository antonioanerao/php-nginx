FROM nginx:alpine

VOLUME [ "/laravel" ]
WORKDIR /laravel

RUN apk update && apk upgrade

ADD build/dev/nginx/default.conf /etc/nginx/conf.d

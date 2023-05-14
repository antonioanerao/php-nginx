FROM nginx:alpine

VOLUME [ "/laravel" ]
WORKDIR /laravel

RUN apk update && apk upgrade

COPY build/prod/nginx/default.conf /etc/nginx/conf.d
COPY ./data /laravel

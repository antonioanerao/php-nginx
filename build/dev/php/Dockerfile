FROM mcr.microsoft.com/mssql-tools as mssql
FROM php:fpm-alpine

VOLUME [ "/laravel" ]
WORKDIR /laravel

COPY --from=mssql /opt/microsoft/ /opt/microsoft/
COPY --from=mssql /opt/mssql-tools/ /opt/mssql-tools/
COPY --from=mssql /usr/lib/libmsodbcsql-13.so /usr/lib/libmsodbcsql-13.so
COPY php/opcache.ini /usr/local/etc/php/conf.d/opcache.ini

RUN apk update && apk add \
		freetype-dev \
		libjpeg-turbo-dev \
		libpng-dev && \
	docker-php-ext-install opcache && \
    docker-php-ext-configure gd --with-freetype --with-jpeg && \
	docker-php-ext-install -j$(nproc) gd

RUN curl -O https://download.microsoft.com/download/1/f/f/1fffb537-26ab-4947-a46a-7a45c27f6f77/msodbcsql18_18.2.1.1-1_amd64.apk && \
    curl -O https://download.microsoft.com/download/1/f/f/1fffb537-26ab-4947-a46a-7a45c27f6f77/mssql-tools18_18.2.1.1-1_amd64.apk && \
    apk add --allow-untrusted msodbcsql18_18.2.1.1-1_amd64.apk && apk add --allow-untrusted mssql-tools18_18.2.1.1-1_amd64.apk && \
    rm msodbcsql18_18.2.1.1-1_amd64.apk && rm mssql-tools18_18.2.1.1-1_amd64.apk

RUN set -xe \
    apk add --no-cache --virtual .persistent-deps freetds unixodbc && \
    apk add --no-cache --virtual .build-deps $PHPIZE_DEPS unixodbc-dev freetds-dev && \
    docker-php-source extract && \
    docker-php-ext-install pdo_dblib && \
    pecl install sqlsrv pdo_sqlsrv && \
    docker-php-ext-enable --ini-name 30-sqlsrv.ini sqlsrv && \
    docker-php-ext-enable --ini-name 35-pdo_sqlsrv.ini pdo_sqlsrv && \
    docker-php-source delete && \
    apk del .build-deps


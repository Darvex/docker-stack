FROM nginx:alpine

MAINTAINER Mahmoud Zalt <mahmoud@zalt.me>

ADD nginx.conf /etc/nginx/

RUN apk update \
    && apk upgrade \
    && apk add --no-cache bash \
    && adduser -D -H -u 1000 -s /bin/bash www-data \
    && apk add --no-cache nano \
    && apk add --no-cache openssl

ARG PHP_UPSTREAM_CONTAINER=php-fpm
ARG PHP_UPSTREAM_PORT=9000

# Create a folder for the keys
RUN mkdir /etc/nginx/ssl 2> /dev/null

# Generate SSL certificates
RUN openssl genrsa -out "/etc/nginx/ssl/localhost.certificate.key" 2048 \
    && openssl req -new -key "/etc/nginx/ssl/localhost.certificate.key" -out "/etc/nginx/ssl/localhost.certificate.csr" -subj "/CN=localhost.certificate/O=YOURCOMPANYNAME/C=UK" \
    && openssl x509 -req -days 365 -in "/etc/nginx/ssl/localhost.certificate.csr" -signkey "/etc/nginx/ssl/localhost.certificate.key" -out "/etc/nginx/ssl/localhost.certificate.crt"

# Set upstream conf and remove the default conf
RUN echo "upstream php-upstream { server ${PHP_UPSTREAM_CONTAINER}:${PHP_UPSTREAM_PORT}; }" > /etc/nginx/conf.d/upstream.conf \
    && rm /etc/nginx/conf.d/default.conf

CMD ["nginx"]

EXPOSE 80 443

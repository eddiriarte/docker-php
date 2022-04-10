FROM ubuntu:20.04

LABEL maintainer="Eduardo Iriarte"

ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=UTC

RUN apt-get update \
    && apt-get install -y gnupg wget curl zip unzip git supervisor sqlite3 \
    && echo "deb http://ppa.launchpad.net/ondrej/php/ubuntu focal main" > /etc/apt/sources.list.d/ppa_ondrej_php.list \
    && apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys E5267A6C \
    && apt-get update \
    && apt-get install -y nginx php8.1-fpm php8.1-cli \
       php8.1-pgsql php8.1-sqlite3 php8.1-gd php8.1-curl \
       php8.1-imap php8.1-mbstring \
       php8.1-xml php8.1-zip php8.1-bcmath php8.1-soap \
       php8.1-intl php8.1-readline php-msgpack php-igbinary php8.1-imagick \
       php8.1-dev php8.1-yaml \
    && php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
    && mkdir -p /run/php \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && echo "daemon off;" >> /etc/nginx/nginx.conf \
    && ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log \
    && sed -i 's/^;daemonize.*$/daemonize = no/g' /etc/php/8.1/fpm/php-fpm.conf \
    && sed -i 's@^error_log.*$@error_log = /proc/self/fd/2@g' /etc/php/8.1/fpm/php-fpm.conf

ADD nginx.conf /etc/nginx/sites-available/default

ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf

ADD start-container.sh /usr/bin/start-container

RUN chmod +x /usr/bin/start-container

ENTRYPOINT ["start-container"]

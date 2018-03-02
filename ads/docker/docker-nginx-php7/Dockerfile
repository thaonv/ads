FROM ubuntu:16.04
MAINTAINER Vinh Le <vinhlt11@fpt.com.vn>

RUN apt-get clean && apt-get update && apt-get install -y locales

# Ensure UTF-8
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8

# Setup timezone & install libraries
RUN echo "Asia/Bangkok" > /etc/timezone \
&& apt-get install -y software-properties-common \
&& apt-get install -y language-pack-en-base \
&& add-apt-repository -y ppa:nginx/stable && add-apt-repository ppa:ondrej/php \
&& apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
    build-essential \
    vim \
    unzip \
    curl \
    wget \
    dialog \
    net-tools \
    git \
    supervisor \
    python-pip \
    nginx \
    php7.1-common \
    php7.1-dev \
    php7.1-fpm \
    php7.1-bcmath \
    php7.1-curl \
    php7.1-gd \
    php7.1-geoip \
    php7.1-imagick \
    php7.1-intl \
    php7.1-json \
    php7.1-ldap \
    php7.1-mbstring \
    php7.1-mcrypt \
    php7.1-memcache \
    php7.1-memcached \
    php7.1-mongo \
    php7.1-mysqlnd \
    php7.1-pgsql \
    php7.1-redis \
    php7.1-sqlite \
    php7.1-xml \
    php7.1-xmlrpc \
    php7.1-zip \
    php7.1-xdebug
RUN \
echo 'deb http://apt.newrelic.com/debian/ newrelic non-free' > /etc/apt/sources.list.d/newrelic.list \
&& curl -sSL https://download.newrelic.com/548C16BF.gpg | apt-key add - \
&& apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y newrelic-php5 \
&& pip install superlance slacker \
&& mkdir /run/php && chown www-data:www-data /run/php \
&& rm -vf /etc/php/7.0/fpm/conf.d/20-xdebug.ini /etc/php/7.0/cli/conf.d/20-xdebug.ini \
&& rm -vf /etc/php/7.0/fpm/conf.d/20-newrelic.ini /etc/php/7.0/cli/conf.d/20-newrelic.ini \
&& rm -vf /var/lib/apt/lists/*.* /tmp/* /var/tmp/* \
&& apt-get autoclean
# Disable xdebug, newrelic by default

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

# Nginx & PHP & Supervisor configuration
COPY conf/nginx/vhost.conf /etc/nginx/sites-available/default
COPY conf/nginx/nginx.conf /etc/nginx/nginx.conf
COPY conf/nginx/api.conf /etc/nginx/conf.d
#COPY conf/php70/php.ini /etc/php/7.0/fpm/php.ini
#COPY conf/php70/cli.php.ini /etc/php/7.0/cli/php.ini
#COPY conf/php70/php-fpm.conf /etc/php/7.0/fpm/php-fpm.conf
#COPY conf/php70/www.conf /etc/php/7.0/fpm/pool.d/www.conf
COPY conf/supervisor/supervisord.conf /etc/supervisord.conf

# Forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log
RUN ln -sf /dev/stderr /var/log/nginx/error.log
RUN mkdir -p /var/log/nginx/kube/api

# Add php test file
COPY ./info.php /src/public/index.php

# Start Supervisord
COPY ./start.sh /start.sh
RUN chmod 755 /start.sh

EXPOSE 80 443

CMD ["/bin/bash", "/start.sh"]

FROM phusion/baseimage:latest
MAINTAINER Cole Turner <turner.cole@gmail.com>

CMD ["/sbin/my_init"]

ENV MEMCACHED_MEMORY_LIMIT 128

# Add Repositories
RUN \
  export LC_ALL=en_US.UTF-8 && \
  export LANG=en_US.UTF-8 && \
  add-apt-repository -y ppa:ondrej/php && \
  add-apt-repository -y ppa:nginx/stable

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb http://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update

# Install Dependencies
RUN \
  DEBIAN_FRONTEND=noninteractive apt-get install -y --allow-downgrades --allow-remove-essential --allow-change-held-packages \
  wget \
  nano \
  libcurl4-openssl-dev \
  libmcrypt-dev \
  libxml2-dev \
  libjpeg-dev \
  libjpeg62 \
  libfreetype6-dev \
  libmysqlclient-dev \
  libgmp-dev \
  libpspell-dev \
  libicu-dev \
  librecode-dev \
  snmp \
  git

# NGINX
RUN \
  apt-get install -y nginx && \
  chown -R www-data:www-data /var/lib/nginx


# Dummy SSL certificates
RUN \
  mkdir -p /etc/nginx/ssl && \
  openssl genrsa -out /etc/nginx/ssl/dummy.key 2048 && \
  openssl req -new -key /etc/nginx/ssl/dummy.key -out /etc/nginx/ssl/dummy.csr -subj "/C=GB/L=London/O=Company Ltd/CN=docker" && \
  openssl x509 -req -days 3650 -in /etc/nginx/ssl/dummy.csr -signkey /etc/nginx/ssl/dummy.key -out /etc/nginx/ssl/dummy.crt

# PHP7
RUN \
  apt-get install -y --allow-downgrades --allow-remove-essential --allow-change-held-packages \
  php7.0 \
  php7.0-cgi \
  php7.0-fpm \
  php7.0-cli \
  php7.0-xsl \
  php7.0-common \
  php7.0-json \
  php7.0-opcache \
  php7.0-mysql \
  php7.0-phpdbg \
  php7.0-intl \
  php7.0-gd \
  php7.0-imap \
  php7.0-ldap \
  php7.0-pgsql \
  php7.0-pspell \
  php7.0-recode \
  php7.0-snmp \
  php7.0-tidy \
  php7.0-dev \
  php7.0-mbstring \
  php7.0-curl

RUN mkdir -p /etc/php/7.0/fpm/addons.d
RUN touch /var/log/php.log

# Yarn
RUN apt-get install -y yarn

# Composer
RUN \
  wget https://getcomposer.org/download/1.0.0-alpha11/composer.phar && \
  chmod +x composer.phar && \
  mv composer.phar /usr/local/bin/composer

# Install Memcached
RUN \
  apt-get install -y --allow-downgrades --allow-remove-essential --allow-change-held-packages \
  memcached \
  php-memcached

RUN sed -i 's/\-m 64/-m $MEMCACHED_MEMORY_LIMIT/g' /etc/memcached.conf


# Configuration
ADD ./etc /etc
ADD ./opt /opt
RUN mkdir -p /app/src && chmod 0775 /app/src

RUN echo export PATH=/usr/sbin:$PATH >> ~/.bashrc
RUN echo export TERM=xterm >> ~/.bashrc

# LOGS
RUN ln -sf /var/log/nginx/access.log /dev/stdout && \
    ln -sf /var/log/nginx/error.log /dev/stderr && \
    ln -sf /var/log/php.log /dev/stderr

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

EXPOSE 80
WORKDIR /app/src

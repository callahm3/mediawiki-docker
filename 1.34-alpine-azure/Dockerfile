FROM alpine:3.11.6

# install dependencies
RUN set -eux; \
	apk update; \
	apk --no-cache add \
		nginx \
		git \
		php7-fpm \
		php7-mcrypt \
		php7-soap \
		php7-openssl \
		php7-gmp \
		php7-pdo_odbc \
		php7-json \
		php7-dom \
		php7-pdo \
		php7-zip \
		php7-mysqli \
		php7-sqlite3 \
		php7-apcu \
		php7-pdo_pgsql \
		php7-bcmath \
		php7-gd \
		php7-odbc \
		php7-pdo_mysql \
		php7-pdo_sqlite \
		php7-gettext \
		php7-xmlreader \
		php7-xmlrpc \
		php7-bz2 \
		php7-iconv \
		php7-pdo_dblib \
		php7-curl \
		php7-ctype \
		php7-cli \
		php7-gd \
		php7-mbstring \
		php7-xml \
		imagemagick \
		supervisor \
		curl \
	;

# configure nginx
COPY config/nginx.conf /etc/nginx/nginx.conf
# Remove default server definition
RUN rm /etc/nginx/conf.d/default.conf

# Configure PHP-FPM
COPY config/fpm-pool.conf /etc/php7/php-fpm.d/www.conf
COPY config/php.ini /etc/php7/conf.d/custom.ini

# Configure supervisord
COPY config/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# doc location
RUN mkdir -p /var/www/html
WORKDIR /var/www/html

# download mediawiki
RUN set -eux; \
	git clone --single-branch  --branch "azure-friendly-1-34-1" https://github.com/callahm3/mediawiki.git; \
	cd mediawiki; \
	git submodule update --init; \
	cd /var/www/html; \
	/bin/cp -rf mediawiki/. . ;\
	rm -rf .git \
	;

# expose ports
EXPOSE 8080

# change ownership
RUN chown -R nginx:nginx /var/www/ && \
	chown -R nginx:nginx /run && \
	chown -R nginx:nginx /var/lib/nginx && \
	chown -R nginx:nginx /var/log/nginx

# su
USER nginx:nginx

RUN ls -la; \
	ls -la /var/www/html; \
	ls -la /var/www; \
	pwd \
	;

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]

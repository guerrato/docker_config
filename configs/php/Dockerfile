FROM php:7-fpm

RUN apt-get update && apt-get install -y libmcrypt-dev
RUN apt-get install -y vim cron supervisor
RUN docker-php-ext-install mbstring pdo_mysql tokenizer mysqli
RUN apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libmcrypt-dev libpng-dev \
	&& docker-php-ext-install -j$(nproc) iconv \
	&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
	&& docker-php-ext-install -j$(nproc) gd \
	&& docker-php-ext-install -j$(nproc) mysqli \
	&& docker-php-ext-enable mysqli

RUN apt-get install -y wget xvfb zlib1g libfontenc1 libxfont1 x11-common xfonts-encodings libfontconfig1 xfonts-utils xfonts-75dpi xfonts-base libxrender1 ucf fonts-dejavu-core ttf-bitstream-vera fonts-liberation fontconfig-config fontconfig libfreetype6 libjpeg62-turbo

RUN wget https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.4/wkhtmltox-0.12.4_linux-generic-amd64.tar.xz \
	&& tar -Jxf wkhtmltox-0.12.4_linux-generic-amd64.tar.xz \
	&& rm -rf wkhtmltox-0.12.4_linux-generic-amd64.tar.xz \
	&& mv wkhtmltox/share/* /usr/local/share/ \
	&& mv wkhtmltox/bin/* /usr/local/bin/ \
	&& mv wkhtmltox/include/* /usr/local/include/ \
	&& mv wkhtmltox/lib/* /usr/local/lib/ \
	&& rm -rf wkhtmltox

RUN apt-get update && apt-get install -y libmagickwand-dev libmagickcore-dev imagemagick \
	&& yes '' | pecl upgrade imagick \
	&& docker-php-ext-enable imagick

ENV QT_XKB_CONFIG_ROOT=/usr/share/X11/xkb

CMD supervisord -c /etc/supervisor/supervisord.conf && supervisorctl reload && php-fpm

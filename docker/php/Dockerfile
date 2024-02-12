#amazonlinux2023をインストール
FROM amazonlinux:2023
# timezoneを東京に
ENV TZ=Asia/Tokyo \
    LANG=ja_JP.UTF-8 \
    LANGUAGE=ja_JP:ja \
    LC_ALL=ja_JP.UTF-8

#php8.2の各種パッケージ,Laravelに必要なライブラリをインストール
RUN dnf update -y && dnf install -y \ 
    php8.2 \
    php8.2-pdo \
    php8.2-mysqlnd \
    php8.2-fpm \
    git \
    zip \
    unzip \
    glibc-langpack-ja 
#remiインストール(この項コメントからapacheまでの#のRUN行全ていらないかも)
#RUN dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
# RUN dnf install -y https://https://rpms.remirepo.net/enterprise/8/php82/aarch64/repoview/php-pecl-grpc.html/php-pecl-grpc-1.61.0-1.el8.remi.8.2.aarch64	
# RUN dnf clean metadata \
#     && dnf install -y \
#     php82 php82-php php82-php-mbstring php82-php-pdo php82-php-xml \
#     php82-php-fpm php82-php-mysqlnd php82-php-gd
# RUN alternatives --install /usr/bin/php php /usr/bin/php82 1
#apacheインストール
RUN dnf install -y httpd httpd-tools httpd-devel httpd-manual
#vimインストール
RUN dnf install -y vim-enhanced

# Composerのインストール
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('sha384', 'composer-setup.php') === 'edb40769019ccf227279e3bdd1f5b2e9950eb000c3233ee85148944e555d97be3ea4f40c3c2fe73b22f875385f6a5155') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /usr/local/bin/composer
ENV COMPOSER_ALLOW_SUPERUSER 1
# ファイルのコピー
COPY ./docker/php/php.ini /etc/php.ini
COPY ./docker/apache/vhost.conf /etc/httpd/conf.d/vhost.conf
COPY ./docker/apache/rewrite.conf /etc/httpd/conf.modules.d/rewrite.conf
#ポート80の解放
EXPOSE 80

#CMD [ "apachectl", "-D", "FOREGROUND" ]
#起動ディレクトリの設定
WORKDIR /var/www/html
RUN httpd -k start
CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
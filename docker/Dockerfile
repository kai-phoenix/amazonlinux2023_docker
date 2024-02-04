#amazonlinux2023をインストール
FROM amazonlinux:2023
#php8.2の各種パッケージをインストール
RUN dnf update && dnf install -y php8.2 git
#apacheインストール
RUN dnf install -y httpd
#vimインストール
RUN dnf install -y vim-enhanced

# Composerのインストール
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === 'e21205b207c3ff031906575712edab6f13eb0b361f2085f1f1237b7126d785e826a450292b6cfd1d64d92e6563bbde02') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer
#ポート80の解放
EXPOSE 80
#起動ディレクトリの設定
WORKDIR /var/www/html
RUN httpd -k start
CMD ["/usr/sbin/httpd", "-DFOREGROUND"]
FROM mysql:8.2
RUN microdnf install yum \ 
    && dnf install -y glibc-langpack-ja glibc-locale-source git

ENV TZ=Asia/Tokyo \
    LANG=ja\_JP.UTF-8

ADD ./docker/mysql/my.cnf /etc/mysql/conf.d/my.cnf
RUN chmod 644 /etc/mysql/conf.d/my.cnf
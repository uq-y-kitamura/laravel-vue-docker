FROM --platform=linux/x86_64 mysql:8.0.23

ENV TZ=UTC

COPY ./docker/mysql/my.cnf /etc/my.cnf

RUN mkdir -p /var/log/mysql && \
    chown mysql:mysql /var/log/mysql
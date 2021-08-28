FROM mariadb:latest

COPY ./config/my.cnf /etc/mysql
RUN chmod a-w /etc/mysql/my.cnf

CMD ["mysqld"]
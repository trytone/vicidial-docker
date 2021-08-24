FROM alpine:latest

ARG VICIDIAL_DB_HOST
ARG VICIDIAL_DB_NAME
ARG VICIDIAL_DB_USER
ARG VICIDIAL_DB_PASSWORD
ARG VICIDIAL_DB_CUSTOM_USER
ARG VICIDIAL_DB_CUSTOM_PASSWORD

EXPOSE 80

# Install depedencies
RUN apk update
RUN apk add --no-cache wget subversion tzdata perl perl-dbi perl-dbd-mysql apache2
RUN apk add --no-cache php5 php5-apache2 php5-mysqli libressl2.7-libcrypto --repository=http://dl-cdn.alpinelinux.org/alpine/v3.8/community --repository=http://dl-cdn.alpinelinux.org/alpine/v3.8/main

# Change timezone
RUN ln -s /usr/share/zoneinfo/Europe/Warsaw /etc/localtime

# Install ViciDial
RUN mkdir -p /usr/src/astguiclient
WORKDIR /usr/src/astguiclient
RUN svn checkout svn://svn.eflo.net:3690/agc_2-X/trunk
WORKDIR /usr/src/astguiclient/trunk
RUN perl install.pl \
--DB_server=$VICIDIAL_DB_HOST \
--DB_database=$VICIDIAL_DB_NAME \
--DB_user=$VICIDIAL_DB_USER \
--DB_pass=$VICIDIAL_DB_PASSWORD \
--DB_custom_user=$VICIDIAL_DB_CUSTOM_USER \
--DB_custom_pass=$VICIDIAL_DB_CUSTOM_PASSWORD \
--web=/var/www/localhost/htdocs \
--no-prompt \
--web-only

RUN echo '<META HTTP-EQUIV=REFRESH CONTENT=\"1; URL=/vicidial/welcome.php\">Please Hold while I redirect you!' > /var/www/localhost/htdocs/index.html

# Copy configuration files
COPY ./httpd.conf /etc/apache2

# Init apache2 deamon
CMD ["-D","FOREGROUND"]
ENTRYPOINT ["/usr/sbin/httpd"]
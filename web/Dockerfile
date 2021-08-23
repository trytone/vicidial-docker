FROM alpine:latest

EXPOSE 80

# Install depedencies
RUN apk update
RUN apk add --no-cache wget subversion perl perl-dbi perl-dbd-mysql apache2
RUN apk add --no-cache php5 php5-apache2 php5-mysqli libressl2.7-libcrypto --repository=http://dl-cdn.alpinelinux.org/alpine/v3.8/community --repository=http://dl-cdn.alpinelinux.org/alpine/v3.8/main

# Install ViciDial
RUN mkdir -p /usr/src/astguiclient
WORKDIR /usr/src/astguiclient
RUN svn checkout svn://svn.eflo.net:3690/agc_2-X/trunk
WORKDIR /usr/src/astguiclient/trunk
RUN perl install.pl --no-prompt --web-only --web-languages --web=/var/www/localhost/htdocs

# Copy configuration files
COPY ./conf/httpd.conf /etc/apache2
COPY ./conf/astguiclient.conf /etc
COPY ./conf/index.html /var/www/localhost/htdocs

# Init apache2 deamon
CMD ["-D","FOREGROUND"]
ENTRYPOINT ["/usr/sbin/httpd"]

# docker build --tag web .
# docker run -d -p 8000:80 --name web web
# docker exec -ti web /bin/sh
# docker rm -f web

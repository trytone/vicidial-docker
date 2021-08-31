FROM alpine:3.5

ARG ASTERISK_SERVER_IP
ARG DB_HOST
ARG DB_NAME
ARG DB_USER
ARG DB_PASS
ARG DB_CUSTOM_USER
ARG DB_CUSTOM_PASS
ARG FTP_HOST
ARG FTP_USER
ARG FTP_PASS

EXPOSE 5060/udp 5060/tcp

RUN apk update
RUN apk add --no-cache gcc g++ make git patch perl perl-dev perl-dbi perl-dbd-mysql curl wget sox lame screen asterisk subversion procps

RUN curl -L http://xrl.us/cpanm > /bin/cpanm && chmod +x /bin/cpanm

RUN cpanm \
MD5 \
Digest::MD5 \
Digest::SHA1 \
readline \
Net::Telnet \
Time::HiRes \
Net::Server \
Switch \
Unicode::Map \
Spreadsheet::WriteExcel \
OLE::Storage_Lite \
IO::Scalar \
Scalar::Util \
Spreadsheet::ParseExcel \
Archive::Zip \
Compress::Raw::Zlib \
Spreadsheet::XLSX \
Test::Tester \
Text::CSV \
Test::NoWarnings \
Text::CSV_PP \
File::Temp \
Text::CSV_XS \
Spreadsheet::Read

WORKDIR /usr/local
RUN wget https://pilotfiber.dl.sourceforge.net/project/vicidial/asterisk-perl-0.08.tar.gz
RUN tar -xf asterisk-perl-0.08.tar.gz
WORKDIR /usr/local/asterisk-perl-0.08
RUN perl Makefile.PL
RUN make all
RUN make install

#RUN cpanm Jcode
#RUN cpanm Proc::ProcessTable
#RUN cpanm Spreadsheet::ReadSXC
#RUN cpanm Mail::Sendmail

# Install ViciDial
RUN mkdir -p /usr/src/astguiclient
WORKDIR /usr/src/astguiclient
RUN svn checkout svn://svn.eflo.net:3690/agc_2-X/trunk
WORKDIR /usr/src/astguiclient/trunk
RUN svn up -r 3261
RUN perl install.pl \
--server_ip=$ASTERISK_SERVER_IP \
--DB_server=$DB_HOST \
--DB_database=$DB_NAME \
--DB_user=$DB_USER \
--DB_pass=$DB_PASS \
--DB_custom_user=$DB_CUSTOM_USER \
--DB_custom_pass=$DB_CUSTOM_PASS \
--FTP_host=$FTP_HOST \
--FTP_user=$FTP_USER \
--FTP_pass=$FTP_PASS \
--REPORT_host=$FTP_HOST \
--REPORT_user=$FTP_USER \
--REPORT_pass=$FTP_PASS \
--no-prompt \
--without-web

RUN mkdir -p /var/lib/asterisk/mohmp3
RUN mkdir -p /var/lib/asterisk/quiet-mp3
RUN mkdir -p /var/lib/asterisk/default

RUN apk add --no-cache dahdi-linux asterisk-dahdi dahdi-linux-grsec

RUN /usr/share/astguiclient/ADMIN_update_server_ip.pl --old-server_ip=10.10.10.15 --server_ip=$ASTERISK_SERVER_IP

CMD ["/bin/sh", "-c", "crond && /usr/share/astguiclient/start_asterisk_boot.pl && /usr/sbin/asterisk -v -f"]

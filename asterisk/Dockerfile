FROM alpine:latest

EXPOSE 5060/udp 5060/tcp

RUN apk update && apk add gcc g++ make git patch perl perl-dev perl-dbi perl-dbd-mysql curl wget sox lame screen asterisk subversion

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

#RUN cpanm DBI
#RUN cpan -f -i DBD::mysql
#RUN cpanm Mail::Sendmail
#RUN cpanm Jcode
#RUN cpanm Proc::ProcessTable
#RUN cpanm Spreadsheet::ReadSXC

# Install ViciDial
RUN mkdir -p /usr/src/astguiclient
WORKDIR /usr/src/astguiclient
RUN svn checkout svn://svn.eflo.net:3690/agc_2-X/trunk
WORKDIR /usr/src/astguiclient/trunk
RUN svn up -r 3261
RUN perl install.pl \
--conffile=/etc/astguiclient.conf \
--home=/usr/share/astguiclient \
--logs=/var/log/astguiclient \
--agi=/var/lib/asterisk/agi-bin \
--web=/var/www/localhost/htdocs \
--sounds=/var/lib/asterisk/sounds \
--monitor=/var/spool/asterisk/monitor \
--DONEmonitor=/var/spool/asterisk/monitorDONE \
--server_ip=$ASTERISK_SERVER_IP \
--DB_server=$DB_HOST \
--DB_database=$DB_NAME \
--DB_user=$DB_USER \
--DB_pass=$DB_PASS \
--DB_custom_user=$DB_CUSTOM_USER \
--DB_custom_pass=$DB_CUSTOM_PASS \
--DB_port=3306 \
--active_keepalives=123456 \
--asterisk_version=13 \
--FTP_host=$FTP_HOST \
--FTP_user=$FTP_USER \
--FTP_pass=$FTP_PASS \
--FTP_port=21 \
--FTP_dir=RECORDINGS \
--REPORT_host=$FTP_HOST \
--REPORT_user=$FTP_USER \
--REPORT_pass=$FTP_PASS \
--REPORT_port=21 \
--REPORT_dir=REPORTS \
--HTTP_path=http://$FTP_HOST \
--fastagi_log_min_servers=3 \
--fastagi_log_max_servers=16 \
--fastagi_log_min_spare_servers=2 \
--fastagi_log_max_spare_servers=8 \
--fastagi_log_max_requests=1000 \
--fastagi_log_checkfordead=30 \
--fastagi_log_checkforwait=60 \
--no-prompt \
--without-web

RUN mkdir -p /var/lib/asterisk/mohmp3
RUN mkdir -p /var/lib/asterisk/quiet-mp3
RUN mkdir -p /var/lib/asterisk/default

CMD ["-v", "-f"]
ENTRYPOINT ["/usr/sbin/asterisk"]

FROM alpine:latest

EXPOSE 5060/udp 5060/tcp

RUN apk update && apk add gcc g++ make git patch perl perl-dev curl wget sox lame screen asterisk

RUN curl -L http://xrl.us/cpanm > /bin/cpanm && chmod +x /bin/cpanm

RUN cpanm MD5
RUN cpanm Digest::MD5	
RUN cpanm Digest::SHA1	
RUN cpanm readline
RUN cpanm DBI
RUN cpanm Net::Telnet
RUN cpanm Time::HiRes
RUN cpanm Net::Server
RUN cpanm Switch
RUN cpanm Unicode::Map
RUN cpanm Spreadsheet::WriteExcel
RUN cpanm OLE::Storage_Lite
RUN cpanm IO::Scalar
RUN cpanm Scalar::Util
RUN cpanm Spreadsheet::ParseExcel
RUN cpanm Spreadsheet::ParseExcel
RUN cpanm Archive::Zip
RUN cpanm Compress::Raw::Zlib
RUN cpanm Spreadsheet::XLSX
RUN cpanm Test::Tester
RUN cpanm Text::CSV
RUN cpanm Test::NoWarnings
RUN cpanm Text::CSV_PP
RUN cpanm File::Temp
RUN cpanm Text::CSV_XS
RUN cpanm Spreadsheet::Read

#RUN cpan -f -i DBD::mysql
#RUN cpanm Mail::Sendmail
#RUN cpanm Jcode
#RUN cpanm Proc::ProcessTable
#RUN cpanm Spreadsheet::ReadSXC

#VOLUME /var/lib/asterisk/sounds /var/lib/asterisk/keys /var/lib/asterisk/phoneprov /var/spool/asterisk /var/log/asterisk
CMD ["/usr/sbin/asterisk", "-vvvdddf", "-T", "-W", "-U", "asterisk", "-p"]
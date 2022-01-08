#Syslog Docker Container with syslog-ng and NFS Share

FROM alpine:latest
WORKDIR /usr/sbin/

RUN   apk add syslog-ng 
RUN mkdir -p /var/log/syslog-ng

#copy syslog config
COPY /config/conf.conf /etc/syslog-ng/conf.d/

EXPOSE 514
CMD ["syslog-ng", "-F"]


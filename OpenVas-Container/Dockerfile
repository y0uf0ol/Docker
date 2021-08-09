FROM alpine:latest

RUN apk add openvas openvas-config gvmd gvm-libs greenbone-security-assistant ospd-openvas postgresql openrc --update --no-cache && \
    
    
    mkdir -p /run/openrc && \
    touch /run/openrc/softlevel  && \
    
    rc-service postgresql setup && \ 
    rc-update add postgresql && \
    rc-service postgresql start && \
    rm -rf /var/cache/apk/* && \

#--- Create and configure the gvm database
    su - postgres && \
    createuser -DRS gvm && \
    createdb -O gvm gvmd && \
    psql gvmd && \
    create role dba with superuser noinherit && \
    grant dba to gvm && \
    create extension if not exists "uuid-ossp" && \
    create extension "pgcrypto" && \
    exit && \
    exit 

EXPOSE 9392/tcp

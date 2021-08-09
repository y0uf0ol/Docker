FROM alpine:latest
LABEL maintainer = "y0uf0ol - https://github.com/y0uf0ol"

RUN apk add postgresql openrc --update --no-cache && \

    mkdir -p /run/openrc && \
    touch /run/openrc/softlevel && \

#--- Postgresql  config
      rc-service postgresql setup && \ 
      rc-update add postgresql && \

    apk add openvas openvas-config gvmd gvm-libs greenbone-security-assistant ospd-openvas --update --no-cache && \
    
   


    #--- Postgresql  start
  
    ping localhost -c 300 && \
    rc-service postgresql start && \

    #--- Create and configure the gvm database
    su - postgres && \
    createuser -DRS gvm && \
    createdb -O gvm gvmd && \
    psql gvmd && \
    create role dba with superuser noinherit && \
    grant dba to gvm && \
    create extension if not exists "uuid-ossp" && \
    create extension "pgcrypto" && \
    \q && \
    exit && \

    #--- GVMd run as gvm user
    su - gvm && \
    gvm-manage-certs -a && \
    exit && \

    #--- Create credentials used to interact with gvmd

    rc-service gvmd start && \
    su - gvm && \
    gvmd --create-user=admin --password=admin && \


    #--- Download the GVM definitions and start GVMd
    
    greenbone-feed-sync --type GVMD_DATA && \
    greenbone-feed-sync --type SCAP && \
    greenbone-feed-sync --type CERT && \
    exit && \

    #--- Add gvmd to start on boot
    rc-update add gvmd && \

    #--- Download NVT definitions
    su - gvm && \
    greenbone-nvt-sync && \


    #--- set IP Configure Greenbone Security Assistant
    echo 'GSAD_LISTEN_ADDRESS="0.0.0.0"' > "/etc/conf.d/gsad" && \

    #--- Start GSAD and add it to default runlevel
    rc-service gsad start && \
    rc-update add gsad && \
    rm -rf /var/cache/apk/*


EXPOSE 9392/tcp

From alpine:latest

RUN apk update && \
    apk add openvas openvas-config gvmd gvm-libs greenbone-security-assistant ospd-openvas && \
   
    #--- Postgresql  Config
    rc-service postgresql setup && \
    rc-service postgresql start && \
    rc-update add postgresql && \

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
    su - gvm && \
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
    echo 'GSAD_LISTEN_ADDRESS="0.0.0.0"' > /etc/conf.d/gsad && \

    #--- Start GSAD and add it to default runlevel
    rc-service gsad start && \
    rc-update add gsad

expose 9392/tcp

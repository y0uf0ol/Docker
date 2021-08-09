FROM alpine:latest

RUN apk add openvas openvas-config gvmd gvm-libs greenbone-security-assistant ospd-openvas postgresql openrc --update --no-cache && \
    
    
    mkdir -p /run/openrc && \
    touch /run/openrc/softlevel  && \
    
    rc-service postgresql setup && \ 
    rc-update add postgresql && \
    rm -rf /var/cache/apk/* && \

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
    echo 'GSAD_LISTEN_ADDRESS="0.0.0.0"' > "/etc/conf.d/gsad" && \

    #--- Start GSAD and add it to default runlevel
    rc-service gsad start && \
    rc-update add gsad && \
    rm -rf /var/cache/apk/*


EXPOSE 9392/tcp


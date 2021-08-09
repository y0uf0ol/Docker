FROM alpine:latest

RUN apk add openvas openvas-config gvmd gvm-libs greenbone-security-assistant ospd-openvas postgresql openrc --update --no-cache && \
    
    
    mkdir -p /run/openrc && \
    touch /run/openrc/softlevel  && \
    
    rc-service postgresql setup && \ 
    rc-update add postgresql && \
    rm -rf /var/cache/apk/* && \

    su - gvm && \
    gvm-manage-certs -a && \
    exit && \

EXPOSE 9392/tcp

FROM alpine:latest

RUN apk update && \
    apk add openvas openvas-config gvmd gvm-libs greenbone-security-assistant ospd-openvas postgresql openrc --no-cache && \
    mkdir -p /run/openrc && \
    touch /run/openrc/softlevel 
    
RUN    rc-service postgresql setup
RUN    rc-update add postgresql 
RUN    rm -rf /var/cache/apk/*

EXPOSE 9392/tcp

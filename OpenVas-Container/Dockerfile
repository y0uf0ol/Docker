FROM alpine:latest

RUN apk update && \
    apk add openvas openvas-config gvmd gvm-libs greenbone-security-assistant ospd-openvas postgresql openrc --no-cache
    
RUN    rc-service postgresql setup
RUN    rc-service postgresql start
RUN    rc-update add postgresql 
RUN    rm -rf /var/cache/apk/*

EXPOSE 9392/tcp

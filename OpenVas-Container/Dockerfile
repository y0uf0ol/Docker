From alpine:latest

RUN apk update && \
    apk add openvas openvas-config gvmd gvm-libs greenbone-security-assistant ospd-openvas && /
    
     #--- Postgresql  Config
    rc-service postgresql setup && \
    rc-service postgresql start && \
    rc-update add postgresql
    rm -rf /var/cache/apk/*

expose 9392/tcp

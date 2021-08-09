FROM alpine:latest

RUN apk update && \
    apk add openvas openvas-config gvmd gvm-libs greenbone-security-assistant ospd-openvas && \
    
    rc-service postgresql setup && \
    rc-service postgresql start && \
    rc-update add postgresql && \
    rm -rf /var/cache/apk/*

EXPOSE 9392/tcp

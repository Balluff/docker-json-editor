FROM alpine:3.8

LABEL Maintainer "Marvin Gfroerer"

LABEL Version "1.1"

RUN apk update && apk upgrade

RUN apk add nodejs apache2 git ca-certificates && rm -rf /var/cache/apk/*

COPY balluff-ssl-cert/cert/balluff_ca.crt /usr/local/share/ca-certificates/balluff_ca.crt

RUN update-ca-certificates

RUN mkdir apachefiles && cd  apachefiles && git clone https://github.com/Balluff/json-editor.git

RUN apk del git && rm -rf /var/cache/apk/* /tmp/*

RUN cp -r /apachefiles/json-editor/* /var/www/localhost/htdocs

RUN mkdir /run/apache2

EXPOSE 80

ENTRYPOINT ["/usr/sbin/httpd","-D","FOREGROUND"]


FROM nginx:1.25-alpine
LABEL maintainer="Son Tran Thanh <286.trants@gmail.com>"

ARG APP_ID=1000

RUN addgroup -g "$APP_ID" app \
 && adduser -G app -u "$APP_ID" -h /var/www -s /bin/bash -S app
RUN touch /var/run/nginx.pid
RUN mkdir /sock

RUN apk add --no-cache \
  curl \
  nss-tools \
  openssl

RUN mkdir /etc/nginx/certs \
  && echo -e "\n\n\n\n\n\n\n" | openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/certs/nginx.key -out /etc/nginx/certs/nginx.crt

ARG TARGETARCH

RUN cd /usr/local/bin/ \
  && curl -L https://github.com/FiloSottile/mkcert/releases/download/v1.4.4/mkcert-v1.4.4-linux-$TARGETARCH -o mkcert \
  && chmod +x mkcert

COPY ./conf/nginx.conf /etc/nginx/
COPY ./conf/default.conf /etc/nginx/conf.d/

RUN mkdir -p /etc/nginx/html /var/www \
  && chown -R app:app /etc/nginx /var/www /var/cache/nginx /var/run/nginx.pid /sock

USER app:app

VOLUME /var/www

WORKDIR /var/www

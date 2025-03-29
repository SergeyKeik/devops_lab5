FROM alpine:latest as builder

ENV NGINX_VERSION=1.23.2

RUN apk add --no-cache \
    build-base \
    pcre-dev \
    zlib-dev \
    openssl-dev \
    wget \
    && rm -rf /var/cache/apk/*

WORKDIR /usr/src

RUN wget http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz \
    && tar -zxvf nginx-${NGINX_VERSION}.tar.gz \
    && rm -f nginx-${NGINX_VERSION}.tar.gz 

WORKDIR /usr/src/nginx-${NGINX_VERSION}

RUN ./configure --prefix=/usr/local/nginx \
                --with-http_ssl_module \
                --with-http_v2_module \
                --with-http_realip_module \
                --with-http_gzip_static_module \
    && make -j$(nproc) && make install

RUN rm -rf /usr/src/nginx-${NGINX_VERSION} \
    && rm -rf /usr/src/*

FROM alpine:latest

RUN apk add --no-cache pcre zlib openssl \
    && rm -rf /var/cache/apk/*  

RUN addgroup -S nginx && adduser -S nginx -G nginx

COPY --from=builder /usr/local/nginx /usr/local/nginx

RUN chown -R nginx:nginx /usr/local/nginx

EXPOSE 80

USER nginx

CMD ["/usr/local/nginx/sbin/nginx", "-g", "daemon off;"]

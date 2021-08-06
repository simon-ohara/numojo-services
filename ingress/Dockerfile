FROM nginx:alpine AS builder

# nginx:alpine contains NGINX_VERSION environment variable, like so:
# ENV NGINX_VERSION 1.15.0
ENV SUBS_VERSION 0.6.4

# Download sources
RUN wget "http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz" -O nginx.tar.gz && \
  wget "https://github.com/yaoweibin/ngx_http_substitutions_filter_module/archive/refs/tags/v${SUBS_VERSION}.tar.gz" -O subs.tar.gz

# For latest build deps, see https://github.com/nginxinc/docker-nginx/blob/master/mainline/alpine/Dockerfile
RUN apk add --no-cache --virtual .build-deps \
  gcc \
  libc-dev \
  make \
  openssl-dev \
  pcre-dev \
  zlib-dev \
  linux-headers \
  libxslt-dev \
  gd-dev \
  geoip-dev \
  perl-dev \
  libedit-dev \
  mercurial \
  bash \
  alpine-sdk \
  findutils

# Reuse same cli arguments as the nginx:alpine image used to build
RUN CONFARGS=$(nginx -V 2>&1 | sed -n -e 's/^.*arguments: //p') \
  mkdir -p /usr/src && \
	tar -zxC /usr/src -f nginx.tar.gz && \
  tar -xzvf "subs.tar.gz" && \
  echo "===================>   $(pwd)" && \
  ls -alF /usr/src && \
  SUBS_DIR="/ngx_http_substitutions_filter_module-${SUBS_VERSION}" && \
  cd /usr/src/nginx-$NGINX_VERSION && \
  ./configure --with-compat $CONFARGS --add-dynamic-module=$SUBS_DIR && \
  make && make install


RUN ls -alF /usr/local/nginx
RUN echo $(find /usr/src/nginx-$NGINX_VERSION -type f -name "*.so" -print)


FROM nginx:alpine
# Extract the dynamic module NCHAN from the builder image
COPY --from=builder /ngx_http_subs_filter_module.so /usr/local/nginx/modules/ngx_http_subs_filter_module.so
# RUN rm /etc/nginx/conf.d/default.conf

COPY nginx.conf /etc/nginx/nginx.conf
# COPY default.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
STOPSIGNAL SIGTERM
CMD ["nginx", "-g", "daemon off;"]


FROM nginx:1.10-alpine

MAINTAINER Pablo Sousa <pablosousa.ads@gmail.com>

ARG UID=1000
ARG GID=1000
ARG CODE_ENV

RUN addgroup -g ${GID:-1000} -S www-data \
 && adduser -u ${UID:-1000} -D -S -G www-data www-data

COPY nginx.conf /etc/nginx/
COPY app.conf /etc/nginx/sites-available/app.conf

RUN mkdir /etc/nginx/sites-enabled/ \
  && ln -s /etc/nginx/sites-available/app.conf /etc/nginx/sites-enabled/app \
  && rm -f /etc/nginx/conf.d/default.conf \
  && echo "upstream php-upstream { server application:9000; }" > /etc/nginx/conf.d/upstream.conf \
  && mkdir /etc/nginx/ssl

RUN chmod -R 0600 /etc/nginx/ssl/

# write permissions to whole tmp for uploads
RUN chmod -R 0777 /tmp

EXPOSE 80
EXPOSE 443

CMD ["nginx"]

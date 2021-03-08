FROM httpd:2.4.46-alpine

WORKDIR /var/www/html

ADD vhost.conf /usr/local/apache2/conf/
RUN echo "ServerName localhost" >> /usr/local/apache2/conf/httpd.conf
RUN echo "Include /usr/local/apache2/conf/vhost.conf" >> /usr/local/apache2/conf/httpd.conf

EXPOSE 80

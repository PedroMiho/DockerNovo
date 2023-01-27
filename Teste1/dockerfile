FROM ubuntu:18.04

RUN apt-get update && apt-get install -y \
    wget \
    apache2 \
    php \
    libapache2-mod-php \
    php-pgsql \
    postgresql-client \
    certbot \
    python-certbot-apache

RUN wget https://wordpress.org/latest.tar.gz && \
    tar xzvf latest.tar.gz && \
    rm latest.tar.gz

RUN mv wordpress /var/www/html/
RUN chown -R www-data:www-data /var/www/html/wordpress

RUN certbot --apache -d example.com

RUN echo "Listen 443" >> /etc/apache2/ports.conf

RUN a2enmod ssl && a2enmod rewrite

COPY wp-config.php /var/www/html/wordpress/

RUN service postgresql start && \
    createuser -U postgres -d -e -E -l -P -r -s wordpress && \
    createdb -U postgres -O wordpress wordpress

COPY start.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/start.sh

CMD ["/usr/local/bin/start.sh"]
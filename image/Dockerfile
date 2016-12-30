FROM lan143/bitbucket-pipelines-php-mysql
MAINTAINER tobias@welante.ch

# SET GIT EOL TO WINDOWS LINE ENDINGS
RUN git config --global core.eol "crlf"

# EXTEND PHP7 WITH PHP-FPM && PHP-XML
RUN apt-get update && apt-get install -y php-fpm php-xml

# ADD USER composer TO RUN COMPOSER JOBS
RUN useradd -ms /bin/bash composer

# DISABLE XDEBUG
RUN echo ';zend_extension=xdebug.so' > /etc/php/7.0/cli/conf.d/20-xdebug.ini

# INSTALL JAVA 8
RUN echo deb http://ftp.de.debian.org/debian jessie-backports main >> /etc/apt/sources.list
RUN apt-get update && apt-get install -y openjdk-8-jre

# INSTALL PHPCS, PHP-CS-FIXER AND PHPUNIT GLOBALLY
RUN curl -LsS https://squizlabs.github.io/PHP_CodeSniffer/phpcs.phar -o /usr/local/bin/phpcs
RUN chmod a+x /usr/local/bin/phpcs
RUN curl -L https://github.com/FriendsOfPHP/PHP-CS-Fixer/releases/download/v1.12.4/php-cs-fixer.phar -o /usr/local/bin/php-cs-fixer
RUN chmod a+x /usr/local/bin/php-cs-fixer
RUN curl -LsS https://phar.phpunit.de/phpunit.phar -o /usr/local/bin/phpunit
RUN chmod a+x /usr/local/bin/phpunit

# INSTALL AND CONFIGURE NGINX
CMD ["/sbin/my_init"]
RUN apt-get update && apt-get install -y nginx

RUN echo "daemon off;" >> /etc/nginx/nginx.conf
RUN ln -sf /dev/stdout /var/log/nginx/access.log
RUN ln -sf /dev/stderr /var/log/nginx/error.log

# CONFIGURE MYSQL
ENV MYSQL_USER=mysql \
    MYSQL_DATA_DIR=/var/lib/mysql \
    MYSQL_RUN_DIR=/run/mysqld \
    MYSQL_LOG_DIR=/var/log/mysql

COPY entrypoint.sh /sbin/entrypoint.sh
RUN chmod 755 /sbin/entrypoint.sh

# EXPOSE PORTS
EXPOSE 81/tcp
EXPOSE 82/tcp
EXPOSE 9000/tcp
EXPOSE 3306/tcp

VOLUME ["${MYSQL_DATA_DIR}", "${MYSQL_RUN_DIR}"]
ENTRYPOINT ["/sbin/entrypoint.sh"]
CMD ["/usr/bin/mysqld_safe"]

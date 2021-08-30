FROM yiisoftware/yii2-php:7.4-apache

# Change document root for Apache
#RUN sed -i -e 's|/app/web|/backend/web|g' /etc/apache2/sites-available/000-default.conf

#RUN mkdir /usr/share/man/man1

ENV TZ=Asia/Jakarta
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

#Change Execution time

RUN cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini
RUN sed -i -e "s/^ *memory_limit.*/memory_limit = 4G/g" -e "s/^ *max_execution_time.*/max_execution_time = 300/g" /usr/local/etc/php/php.ini

RUN sed -i -e "s/^ *memory_limit.*/memory_limit = 4G/g" -e "s/^ *max_execution_time.*/max_execution_time = 300/g" /usr/local/etc/php/conf.d/base.ini
RUN sed -i -e "s/^ *upload_max_filesize.*/upload_max_filesize = 2048M/g" -e "s/^ *post_max_size.*/post_max_size = 2048M/g" /usr/local/etc/php/conf.d/base.ini


RUN apt-get update
#RUN apt-get install msttcorefonts -y
#RUN apt install ttf-mscorefonts-installer
RUN apt install apt-transport-https ca-certificates wget dirmngr gnupg software-properties-common -y
RUN wget -qO - https://adoptopenjdk.jfrog.io/adoptopenjdk/api/gpg/key/public | apt-key add -
RUN add-apt-repository --yes https://adoptopenjdk.jfrog.io/adoptopenjdk/deb/
RUN apt-get update
RUN apt install adoptopenjdk-8-hotspot -y

#Install PHP-LDAP Extention
RUN \
    apt-get update && \
    apt-get install libldap2-dev -y && \
    rm -rf /var/lib/apt/lists/* && \
    docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && \
    docker-php-ext-install ldap

#Install Msfonts
RUN apt-get update && apt-get install  cabextract libfontenc1 libmspack0 xfonts-encodings xfonts-utils -y
RUN wget  -P /var/tmp http://ftp.br.debian.org/debian/pool/contrib/m/msttcorefonts/ttf-mscorefonts-installer_3.7_all.deb && dpkg -i /var/tmp/ttf-mscorefonts-installer_3.7_all.deb
RUN apt --fix-broken install

RUN rm  /var/tmp/ttf-mscorefonts-installer_3.7_all.deb


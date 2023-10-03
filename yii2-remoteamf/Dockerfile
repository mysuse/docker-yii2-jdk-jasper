FROM yiisoftware/yii2-php:7.4-apache

# Change document root for Apache
#RUN sed -i -e 's|/app/web|/backend/web|g' /etc/apache2/sites-available/000-default.conf

#RUN mkdir /usr/share/man/man1
RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends tzdata curl ca-certificates fontconfig locales \
    && echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
    && locale-gen en_US.UTF-8 \
    && rm -rf /var/lib/apt/lists/*

ENV JAVA_VERSION jdk8u292-b10

RUN set -eux; \
    ARCH="$(dpkg --print-architecture)"; \
    case "${ARCH}" in \
       aarch64|arm64) \
         ESUM='a29edaf66221f7a51353d3f28e1ecf4221268848260417bc562d797e514082a8'; \
         BINARY_URL='https://github.com/AdoptOpenJDK/openjdk8-binaries/releases/download/jdk8u292-b10/OpenJDK8U-jdk_aarch64_linux_hotspot_8u292b10.tar.gz'; \
         ;; \
       armhf|armv7l) \
         ESUM='0de107b7df38314c1daab78571383b8b39fdc506790aaef5d870b3e70048881b'; \
         BINARY_URL='https://github.com/AdoptOpenJDK/openjdk8-binaries/releases/download/jdk8u292-b10/OpenJDK8U-jdk_arm_linux_hotspot_8u292b10.tar.gz'; \
         ;; \
       ppc64el|ppc64le) \
         ESUM='7ecf00e57033296fd23201477a64dc13a1356b16a635907e104d079ddb544e4b'; \
         BINARY_URL='https://github.com/AdoptOpenJDK/openjdk8-binaries/releases/download/jdk8u292-b10/OpenJDK8U-jdk_ppc64le_linux_hotspot_8u292b10.tar.gz'; \
         ;; \
       s390x) \
         ESUM='276a431c79b7e94bc1b1b4fd88523383ae2d635ea67114dfc8a6174267f8fb2c'; \
         BINARY_URL='https://github.com/AdoptOpenJDK/openjdk8-binaries/releases/download/jdk8u292-b10/OpenJDK8U-jdk_s390x_linux_hotspot_8u292b10.tar.gz'; \
         ;; \
       amd64|x86_64) \
         ESUM='0949505fcf42a1765558048451bb2a22e84b3635b1a31dd6191780eeccaa4ada'; \
         BINARY_URL='https://github.com/AdoptOpenJDK/openjdk8-binaries/releases/download/jdk8u292-b10/OpenJDK8U-jdk_x64_linux_hotspot_8u292b10.tar.gz'; \
         ;; \
       *) \
         echo "Unsupported arch: ${ARCH}"; \
         exit 1; \
         ;; \
    esac; \
    curl -LfsSo /tmp/openjdk.tar.gz ${BINARY_URL}; \
    echo "${ESUM} */tmp/openjdk.tar.gz" | sha256sum -c -; \
    mkdir -p /opt/java/openjdk; \
    cd /opt/java/openjdk; \
    tar -xf /tmp/openjdk.tar.gz --strip-components=1; \
    rm -rf /tmp/openjdk.tar.gz;

ENV JAVA_HOME=/opt/java/openjdk \
    PATH="/opt/java/openjdk/bin:$PATH"
    

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
#RUN wget -qO - https://adoptopenjdk.jfrog.io/adoptopenjdk/api/gpg/key/public | apt-key add -
#RUN add-apt-repository --yes https://adoptopenjdk.jfrog.io/adoptopenjdk/deb/
#RUN apt-get update
#RUN apt install adoptopenjdk-8-hotspot -y

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

RUN apt install python3 python3-pip -y
RUN pip install pika
RUN pip install pytz

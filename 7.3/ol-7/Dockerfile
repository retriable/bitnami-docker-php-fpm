FROM bitnami/oraclelinux-runtimes:7-r394
LABEL maintainer "Bitnami <containers@bitnami.com>"

# Install required system packages and dependencies
RUN install_packages bzip2-libs ca-certificates cyrus-sasl-lib freetds freetype glibc gmp gnutls keyutils-libs krb5-libs libcom_err libcurl libffi libgcc libgcrypt libgpg-error libicu libidn libjpeg-turbo libmemcached libpng libselinux libssh2 libstdc++ libtasn1 libtidy libxml2 libxslt ncurses-libs nettle nspr nss nss-softokn-freebl nss-util openldap openssl-libs p11-kit pcre postgresql-libs readline wget xz-libs zlib
RUN wget -nc -P /tmp/bitnami/pkg/cache/ https://downloads.bitnami.com/files/stacksmith/php-7.3.8-0-linux-x86_64-ol-7.tar.gz && \
    echo "0cb38be663ea54976a58f46e1dd1201727a81798b88a5c5b2163c4c39921d32d  /tmp/bitnami/pkg/cache/php-7.3.8-0-linux-x86_64-ol-7.tar.gz" | sha256sum -c - && \
    tar -zxf /tmp/bitnami/pkg/cache/php-7.3.8-0-linux-x86_64-ol-7.tar.gz -P --transform 's|^[^/]*/files|/opt/bitnami|' --wildcards '*/files' && \
    rm -rf /tmp/bitnami/pkg/cache/php-7.3.8-0-linux-x86_64-ol-7.tar.gz

ENV BITNAMI_APP_NAME="php-fpm" \
    BITNAMI_IMAGE_VERSION="7.3.8-ol-7-r8" \
    PATH="/opt/bitnami/php/bin:/opt/bitnami/php/sbin:$PATH"

EXPOSE 9000

ENV IMAGEMAGICK_VERSION=7.0.8-59 IMAGICK_VERSION=3.4.4
RUN install_packages gcc make autoconf
RUN wget ftp://ftp.imagemagick.org/pub/ImageMagick/ImageMagick-$IMAGEMAGICK_VERSION.tar.gz && tar -zxf ImageMagick-$IMAGEMAGICK_VERSION.tar.gz && cd ImageMagick-$IMAGEMAGICK_VERSION && ./configure --prefix=/usr/local/imagemagick && make && make install && cd .. && rm -rf ImageMagick-$IMAGEMAGICK_VERSION*
RUN wget http://pecl.php.net/get/imagick-$IMAGICK_VERSION.tgz && tar -zxf imagick-$IMAGICK_VERSION.tgz && cd imagick-$IMAGICK_VERSION && phpize && ./configure --with-php-config=/opt/bitnami/php/bin/php-config --with-imagick=/usr/local/imagemagick && make && make install && cd .. && rm -rf imagick-$IMAGICK_VERSION*

WORKDIR /app
CMD [ "php-fpm", "-F", "--pid", "/opt/bitnami/php/tmp/php-fpm.pid", "-y", "/opt/bitnami/php/etc/php-fpm.conf" ]

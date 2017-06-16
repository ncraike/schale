FROM php:7.0
MAINTAINER Studio None <developers@studionone.com.au>
LABEL \
    project="studionone/shale" \
    maintainer="Studio None <developers@studionone.com.au>" \
    author="Studio None <developers@studionone.com.au>" \
    homepage="https://github.com/studionone/shale" \
    git_repository="https://github.com/studionone/shale.git"

ENV PROJECT_DIR /root/shale
ENV DEBIAN_FRONTEND noninteractive
ENV TERM xterm-256color

#
# Install OS-wide packages and PHP extensions
#
RUN \
    apt-get update \
    && pecl channel-update pecl.php.net \
    && apt-get install -y \
        libzip-dev \
    && pecl install \
        xdebug \
    && docker-php-ext-install -j$(nproc) \
        zip \
    && docker-php-ext-enable \
        xdebug

#
# Setup install directory for Shale
#
RUN mkdir -p "$PROJECT_DIR"
WORKDIR $PROJECT_DIR

#
# Install composer, use it to install PHP dependencies
#
COPY composer.json $PROJECT_DIR/composer.json
RUN \
    curl --output /usr/local/bin/composer https://getcomposer.org/composer.phar \
    && chmod +x /usr/local/bin/composer \
    && /usr/local/bin/composer install --no-interaction
#
# We leave this to last, so changes to src or tests during active
# development don't trigger a re-run of composer install
#
COPY src $PROJECT_DIR/src
COPY tests $PROJECT_DIR/tests

#
# Set default command to run our test suite
#
# This is used if no command is given after:
#
#     docker run <options> <image-name>
#
CMD composer test

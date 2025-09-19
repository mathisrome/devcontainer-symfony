FROM php:8.3-fpm

## Configuration de la Timezone en France
RUN rm /etc/localtime && ln -s /usr/share/zoneinfo/Europe/Paris /etc/localtime

## PHP Extensions & Composer
COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/
RUN install-php-extensions gd \
    exif \
    pdo_mysql \
    intl \
    zip \
    bcmath \
    @composer

RUN mkdir /var/www/.composer && chown -R www-data:www-data /var/www/.composer

## Symfony CLI
RUN curl -sS https://get.symfony.com/cli/installer | bash \
    && mv /root/.symfony*/bin/symfony /usr/local/bin/symfony \
    && symfony server:ca:install

## DEV KIT
## Phpcs
RUN curl -OL https://squizlabs.github.io/PHP_CodeSniffer/phpcs.phar \
    && mv phpcs.phar /usr/local/bin/phpcs \
    && chmod +x /usr/local/bin/phpcs \
    ## Phpcbf
    && curl -OL https://squizlabs.github.io/PHP_CodeSniffer/phpcbf.phar \
    && mv phpcbf.phar /usr/local/bin/phpcbf \
    && chmod +x /usr/local/bin/phpcbf \
    ## Grumphp
    && curl -OL https://github.com/phpro/grumphp/releases/download/v2.15.0/grumphp.phar \
    && mv grumphp.phar /usr/local/bin/grumphp \
    && chmod +x /usr/local/bin/grumphp

## Phpstan
COPY --from=ghcr.io/phpstan/phpstan:latest-php8.3 /composer/vendor/phpstan/phpstan/phpstan.phar .
RUN mv phpstan.phar /usr/local/bin/phpstan \
    && chmod +x /usr/local/bin/phpstan

COPY ./config/project.ini /usr/local/etc/php/conf.d/project.ini

WORKDIR /workspace
ARG PHP_VERSION
FROM ${PHP_VERSION}

ARG TZ
ARG PHP_EXTENSIONS
ARG CONTAINER_PACKAGE_URL

# 配置清华镜像
RUN if [ $CONTAINER_PACKAGE_URL ] ; then sed -i "s/dl-cdn.alpinelinux.org/${CONTAINER_PACKAGE_URL}/g" /etc/apk/repositories ; fi

# 安装php扩展
COPY ./extensions /tmp/extensions
WORKDIR /tmp/extensions
RUN chmod +x install.sh \
    && sh install.sh \
    && rm -rf /tmp/extensions

# 添加扩展安装执行脚本
ADD ./extensions/install-php-extensions  /usr/local/bin/

# 赋予可执行权限
RUN chmod uga+x /usr/local/bin/install-php-extensions

# 安装必要的软件
# supervisor
# bash
# git
# 设置时区
RUN apk update \
    && apk upgrade \
    && apk add supervisor \
    && apk add bash \
    && apk add git \
    && apk --no-cache add tzdata \
    && cp "/usr/share/zoneinfo/$TZ" /etc/localtime \
    && echo "$TZ" > /etc/timezone \
    && rm -rf /var/cache/apk/*

# Fix: https://github.com/docker-library/php/issues/240
RUN apk add gnu-libiconv libstdc++ --no-cache --repository http://${CONTAINER_PACKAGE_URL}/alpine/edge/community/ --allow-untrusted
ENV LD_PRELOAD /usr/lib/preloadable_libiconv.so php


# 安装 composer 并更改它的缓存主页
RUN curl -o /usr/bin/composer https://mirrors.aliyun.com/composer/composer.phar \
    && chmod +x /usr/bin/composer
ENV COMPOSER_HOME=/tmp/composer

# 配置用户组
RUN apk --no-cache add shadow && usermod -u 1000 www-data && groupmod -g 1000 www-data

# 设置工作目录
WORKDIR /var/www

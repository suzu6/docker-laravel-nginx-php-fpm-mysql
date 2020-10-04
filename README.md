# Laravel + Vue のサンプル

[Vue + Vue Router + Vuex + Laravelで写真共有アプリを作ろう](https://www.hypertextcandy.com/vue-laravel-tutorial-introduction/)をなぞって作成しています。


docker-compose up -d

docker-compose down --rmi all --volumes

## 

```log
checking for libzip >= 0.11... no
configure: error: Package requirements (libzip >= 0.11) were not met:

No package 'libzip' found

Consider adjusting the PKG_CONFIG_PATH environment variable if you
installed software in a non-standard prefix.

Alternatively, you may set the environment variables LIBZIP_CFLAGS
and LIBZIP_LIBS to avoid the need to call pkg-config.
See the pkg-config man page for more details.
ERROR: Service 'php' failed to build : The command '/bin/sh -c apt-get update   && apt-get install -y wget git unzip libpq-dev   && : 'Install Node.js'   &&  curl -sL https://deb.nodesource.com/setup_12.x | bash -   && apt-get install -y nodejs   && : 'Install PHP Extensions'   && apt-get install -y zlib1g-dev mariadb-client   && docker-php-ext-install zip pdo_mysql   && : 'Install Composer'   && chmod 755 /install-composer.sh   && /install-composer.sh   && mv composer.phar /usr/local/bin/composer' returned a non-zero code: 1
```

php:7.4-fpmだとdocker-php-ext-install でphp拡張をインストールする際に`libzip`を使うようで`No package 'libzip' found`が出た。

https://qiita.com/khara_nasuo486/items/1aa40d26efb3eb9a5a04


# E: Unable to locate package oniguruma
> mbstring に替わり libonig-dev というパッケージを追加する必要があるそう。
https://qiita.com/June8715/items/1df5958b95c1ff2d4c1b
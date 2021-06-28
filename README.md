# LaravelのDocker環境

構成とバージョン。
ここでは具体的なバージョンを書いていますが、基本的に環境構築時の各最新バージョンになります。
- php (php-fpm) 7.4.1
  - composer 1.10.13
  - nodejs v12.18.4
  - npm 6.14.6
- php80 (php-fpm) 8.0.7
  - composer 2.1.3
  - nodejs v16.4.0
  - npm 7.18.1
- mysql 8.0 (5.7も可能)
  - パスワードなど初期設定をする。
- nginx 1.19.2
  - Laravel向けのconfを用意する。

詳細は[ブログ](https://www.suzu6.net/posts/254-laravel-docker-compose/)に書いた。

## ディレクトリ構成
```sh
/project
├─ /docker    # コンテナ内のデータ保存先
|  ├─ /db
|  |  ├─ /data           # データベース保存先
|  |  ├─ /sql
|  |  └─ /my.conf        # mysqlの設定
|  ├─ /nginx
|  |  └─ /default.conf   # Nginxの設定
|  └─ /php
|     ├─ /Dockerfile     # phpコンテナの設定
|     └─ /php.ini        # phpの設定
├─ /web       # アプリケーションコード
└─ docker-compose.yml
``` 

## docker-composeのコマンド
ホスト側でよく使うコマンドです。

```sh
# コンテナ起動
$ docker-compose up -d
# http://localhost/ で確認できる。

# イメージの情報を表示
$ docker-compose images
Container   Repository    Tag       Image Id       Size
---------------------------------------------------------
db          mysql        8.0      2933adc350f3   546.1 MB
nginx       nginx        latest   35c43ace9216   133.1 MB
php         docker_php   latest   c589f52d0377   790.5 MB

# サービスのログを出力します(phpの例)
$ docker-compose logs php
Attaching to php
php      | [22-Feb-2021 20:39:06] NOTICE: fpm is running, pid 1      
php      | [22-Feb-2021 20:39:06] NOTICE: ready to handle connections

# コンテナ削除
$ docker-compose down

# コンテナ、イメージ、ボリューム、ネットワークを一括完全消去
$ docker-compose down --rmi all --volumes

# phpコンテナに接続する（コンテナ名を変えて他のコンテナに接続できる）
$ docker-compose exec php bash
```

## バージョン確認
```bash
$ docker-compose exec php bash
root@4e385c355d92:/var/www#

# php -v
PHP 8.0.7 (cli) (built: Jun 23 2021 12:34:03) ( NTS )
Copyright (c) The PHP Group
Zend Engine v4.0.7, Copyright (c) Zend Technologies

# node -v
v16.4.0
# npm -v
7.18.1
# composer -V
   ______
  / ____/___  ____ ___  ____  ____  ________  _____
 / /   / __ \/ __ `__ \/ __ \/ __ \/ ___/ _ \/ ___/
/ /___/ /_/ / / / / / / /_/ / /_/ (__  )  __/ /
\____/\____/_/ /_/ /_/ .___/\____/____/\___/_/
                    /_/
Composer version 2.1.3 2021-06-09 16:31:20

# コンテナから出るコマンド
# exit
```

## install laravel 

```bash
composer create-project laravel/laravel laravel
```

## php.ini
[PHP7.4 ぼくのかんがえたさいきょうのphp.ini](https://qiita.com/ucan-lab/items/0d74378e1b9ba81699a9)を参考にした。

## laravel .env

LaravelからDBに接続するための.envの設定。
```conf
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=database
DB_USERNAME=docker
DB_PASSWORD=docker
```
`DB_HOST`はlocalhostではなくコンテナ名にすることに注意する。
# LaravelのDocker環境

構成とバージョン。最新になるようにしています。
- php (php-fpm) 7.4.1
  - composer 1.10.13
  - nodejs v12.18.4
  - npm 6.14.6
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
root@41b99e6e8ed6:/var/www#

# php -v
PHP 8.0.2 (cli) (built: Feb  9 2021 19:20:59) ( NTS )
Copyright (c) The PHP Group
Zend Engine v4.0.2, Copyright (c) Zend Technologies

# node -v
v15.9.0
# composer -V
Composer version 2.0.9 2021-01-27 16:09:27

# 出る
# exit
```

## php.ini
[PHP7.4 ぼくのかんがえたさいきょうのphp.ini](https://qiita.com/ucan-lab/items/0d74378e1b9ba81699a9)を参考にした。
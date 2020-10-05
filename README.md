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

# コンテナ削除
$ docker-compose down

# コンテナ、イメージ、ボリューム、ネットワークを一括完全消去
$ docker-compose down --rmi all --volumes

# phpコンテナに接続する（コンテナ名を変えて他のコンテナに接続できる）
$ docker-compose exec php bash
```
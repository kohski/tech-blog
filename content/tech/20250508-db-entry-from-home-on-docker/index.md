---
title: '『おうちで学べるデータベースのきほん』第2版のコマンドをDockerで行うメモ'
tags: ["database", "rdb", "book review", "docker", "mysql"] # タグにDockerとMySQLを追加
# description: "" # 個別ページのタイトル下部に表示される
slug: "db-entry-from-home-on-docker"
summary: "『おうちで学べるデータベースのきほん』第2版のコマンドをDockerで行うためのメモです。" # 
cover:
    image: "<image path/url>" # 300 * 180を基準にしたい
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
draft: false
# ----------------
date: 2025-05-08T10:13:40+09:00
author: "kohski"
showToc: true
TocOpen: false
hidemeta: false
comments: false
# canonicalURL: "https://canonical.url/to/page"
disableHLJS: false # to disable highlightjs
disableShare: false
hideSummary: false
searchHidden: true
ShowReadingTime: false
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: false
ShowRssButtonInSectionTermList: true
UseHugoToc: true
---

# はじめに

[『おうちで学べるデータベースのきほん』第2版](https://kohski.dev/tech/db-entry-from-home/)を読んだわけですが、その中で紹介されていたのが、ローカルに直接MySQLをインストールするか、EC2上に環境設定するかの2つの方法でした。

実際に環境をセットアップすることが勉強になるということは十分に理解したものの、
前者は自分のPCで実行するには少し気が引けますし、後者は面倒臭いということで、Dockerで動かすためのメモを記載します。この方法であれば、PC環境を汚さずに簡単にMySQLの環境を構築できます。

# 前提条件

```
$ sw_vers
ProductName:            macOS
ProductVersion:         14.1
BuildVersion:           23B2073

$ uname -m
arm64

$ docker -v
Docker version 25.0.4-rd, build c4cd0a9
```

# 手順

## docker-composeで環境構築

1. docker-compose.yamlを作成
    ```yaml
    version: '3.8'

    services:
      mysql:
        image: mysql:8.4
        platform: linux/arm64
        container_name: mysql8.4
        environment:
          MYSQL_ROOT_PASSWORD: <任意のパスワード>
        ports:
          - "3306:3306"
        volumes:
          - mysql_data:/var/lib/mysql

    volumes:
      mysql_data:
    ```

2. コンテナを起動
    ```bash
    $ docker compose up -d
    ```

## サンプルデータの準備

1. sakilaデータベースのインポート
    ```bash
    $ curl -L -O http://downloads.mysql.com/docs/sakila-db.zip
    $ unzip sakila-db.zip
    $ docker cp sakila-db/. mysql8.4:/tmp/
    $ docker exec -it mysql8.4 mysql -uroot -p<docker-compose.yamlで決めたパスワード> -e "source /tmp/sakila-schema.sql"
    $ docker exec -it mysql8.4 mysql -uroot -p<docker-compose.yamlで決めたパスワード> -e "source /tmp/sakila-data.sql"
    ```

2. worldデータベースのインポート
    ```bash
    $ curl -L -O https://downloads.mysql.com/docs/world-db.zip
    $ unzip world-db.zip
    $ docker cp world-db/world.sql mysql8.4:/tmp/
    $ docker exec -it mysql8.4 mysql -uroot -p<docker-compose.yamlで決めたパスワード> -e "source /tmp/world.sql"
    ```

## MySQLコンテナ内に入る

```bash
$ docker exec -it mysql8.4 mysql -uroot -p<docker-compose.yamlで決めたパスワード> 
```

接続後は以下のようにデータベースが利用可能か確認できます：

```sql
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| world              |
+--------------------+
6 rows in set (0.00 sec)

mysql> USE sakila;
mysql> SHOW TABLES;
```

## 環境の片付け方

使い終わったらコンテナを停止・削除するには以下のコマンドを実行します：

```bash
$ docker compose down
```

ボリュームも含めて完全に削除する場合：

```bash
$ docker compose down -v
```

# 最後に

この手順で作った環境で『おうちで学べるデータベースのきほん』第2版は最後まで手を動かしながら読むことができました。Dockerを使うことで環境構築の手間を減らしつつ、クリーンな環境でデータベースの学習を進めることができるのでおすすめです。
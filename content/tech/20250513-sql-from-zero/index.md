---
title: '『SQL 第2版 ゼロからはじめるデータベース操作』を読んだ感想と学び'
tags: ["database", "rdb", "book review", "データベース設計"] # Lambda, API Gateway, AWS, Python
# description: "『SQL 第2版 ゼロからはじめるデータベース操作』を読んで学んだことと実践環境の構築について" # 個別ページのタイトル下部に表示される
slug: "sql-zero-to-hero-book-review"
summary: "ミックさん著『SQL 第2版 ゼロからはじめるデータベース操作』を読んでSQLの基礎知識を学んだ" # 
cover:
    image: "<image path/url>" # 300 * 180を基準にしたい
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
draft: false
# ----------------
date: 2025-05-13T11:05:34+09:00
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

ミックさんのデータベース学習ロードマップの3冊目を読みました。
https://x.com/copinemickmack/status/1871863629706096770/photo/1

前回の[『達人に学ぶDB設計徹底指南書 第2版』から学んだRDB設計の要点](https://kohski.dev/tech/db-design-instruction-from-expert/)では、設計について理解を深めましたが、理論ばかりで実践が足りませんでした。今回はしっかりと手を動かして知識の定着を目指したいという狙いがありました。

| 書籍情報 | 内容 |
| --- | --- |
| 書籍名 | SQL 第2版 ゼロからはじめるデータベース操作 |
| 著者名 | ミック |
| 出版年 | 2016/6/16 |
| ISBN | 9784798144450 |
| 書籍ページ | https://www.shoeisha.co.jp/book/detail/9784798144450 |


# 環境のセットアップ

本書ではローカルにPostgreSQLをダウンロードして環境を作ることを勧められていましたが、ローカル環境を汚したくなかったのでDockerを使用して学習環境を構築しました。

Chapter 9は読み飛ばしたのでわかりませんが、他の章ではこの環境で問題なく学習できました。

## 実践環境の構築手順

1. docker-compose.yml
    ```yaml
    version: '3.8'

    services:
        postgres:
        image: postgres:14
        container_name: postgres-db
        restart: always
        ports:
            - "5432:5432"
        environment:
            POSTGRES_USER: postgres
            POSTGRES_PASSWORD: postgres
            POSTGRES_DB: postgres
        volumes:
            - postgres_data:/var/lib/postgresql/data
            - ./init.sql:/docker-entrypoint-initdb.d/init.sql

    volumes:
        postgres_data: 
    ```
    
2. Dockerコンテナの起動
    
    ```bash
    docker compose up -d 
    ```
    
3. PostgreSQLに接続
    
    ```bash
    docker exec -it postgres-db psql -U postgres
    ```

# 学んだこと

細かい基礎的なクエリについては書籍に譲りますが、特に印象に残ったポイントを紹介します。

## SELECT文の実行順序

SELECT文には各種制約や複雑な仕様がありますが、以下の点を理解することで大きく整理できました。

- 記述順序
    1. SELECT
    2. FROM
    3. WHERE
    4. GROUP BY
    5. HAVING
    6. ORDER BY
- 処理順序
    1. FROM
    2. WHERE
    3. GROUP BY
    4. HAVING
    5. SELECT
    6. ORDER BY

記述順序と処理順序が異なる点が重要であり、特にWHERE句とHAVING句の使い分けなどを理解する際に役立ちました。

## NULLの扱いが重要

SQLにおけるNULLの扱いは非常に重要です。以下のポイントが特に学びになりました：

- 集約関数では自動的にNULLを無視してくれる
- 3値論理（TRUE/FALSE/UNKNOWN）の考え方
- NULLを検索する場合は `IS NULL` 述語を使用する必要がある
- NULLが関わる演算結果はほとんどの場合NULLになる

これらを踏まえると、以下が重要だと実感しました：

- データベース設計時点でできるだけNULLを排除すること
- クエリ作成時にNULLの存在を常に意識すること

## 判断基準が明確で実用的

本書の優れている点として、以下のような判断に迷う場面で明確な基準を示してくれることが挙げられます：

- DISTINCT vs GROUP BY
- IN述語 vs EXISTS述語
- WHERE句 vs HAVING句

「原則や設計思想に立ち返るとこうすべき」という著者の説明が随所にあり、非常に参考になりました。

# 今後の展望

ミックさんの書籍を引き続き学習していく予定ですが、同時に実際のアプリケーション開発における効率的なデータベース活用についても調査したいと考えています。

具体的には：
- ORMやクエリビルダーの実践的な活用方法
- DDL、DCLについてのより深い理解（本書では十分に練習できなかった）
- インデックス設計などのパフォーマンスチューニング

実際の開発現場で役立つスキルとして、これらの知識を深めていきたいと思います。
---
title: '『Grokking Relational Database Design』を読んで論理設計の理解を深めた'
tags: ["database", "rdb", "book review", "データベース設計"]
# description: "" # 個別ページのタイトル下部に表示される
slug: "grokking-relational-database-design"
summary: "『Grokking Relational Database Design』を読んで論理設計の理解を深めた。" # 
cover:
    image: "<image path/url>" # 300 * 180を基準にしたい
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
draft: false
# ----------------
date: 2025-05-29T13:17:06+09:00
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

ミックさん本をローリングしていく中で、SQLや物理設計と論理設計の一部については理解が深まりました。
今回はさらに論理設計の中でもモデリングをどのように進めていくのかという点に着目して、『Grokking Relational Database Design』を読みました。

| 書籍情報 | 内容 |
| --- | --- |
| 書籍名 | Grokking Relational Database Design |
| 著者名 | Qiang Hao, Michail Tsikerdekis |
| 出版年 | 2025年3月 |
| ISBN | 9781633437418 |
| 書籍ページ | https://www.manning.com/books/grokking-relational-database-design |

# ざっくりどんな本だったか

「The Sci-Fi Collective」という架空のSF商品の販売サイトのデータベースをモデリングしていく内容です。

Chapter1, Chapter2でRDBMSの総論的な説明や最低限のSQLの説明があります。

Chapter3からはデータベース設計の全体像が示されます
1. 要求の収集
2. 分析と設計
3. 実装・統合・テスト

1. については、架空の要件やインタビューのサマリが提供され、Chapter3の中で1.までは完了します。

Chapter4ではテーブル設計を本格的に進めます

1. 要求の収集のフェーズで収集した情報を元にEntityとAttributeを決定
2. テーブルにキーを決める
3. またAttributeに対してデータタイプを決めます
   - 文字列（CHAR, VARCHAR, TEXT）
   - 数値（INT, DECIMAL）
   - 日時（DATETIME, TIMESTAMP）
   など、適切なデータ型の選択

Chapter5ではテーブル間のリレーションを設定します

1. テーブル間のカーディナリティを定める
   - 1対1（1:1）
   - 1対多（1:N）
   - 多対多（M:N）
   の関係を明確にします

2. 強いエンティティ、弱いエンティティの観点でキー設計を見直し
   - 強いエンティティ：独立して存在できる
   - 弱いエンティティ：親エンティティに依存する

特に強いエンティティ、弱いエンティティという概念は初耳だったので参考になりました。
この概念は、テーブル間の依存関係を明確にし、適切なキー設計を行う上で重要な視点を提供してくれます。

Chapter6では正規化と実装を進めます
1. 正規化を進めます
    - 1NF
    - 2NF
    - 3NF
    - BCNF
2. 要件に応じて各種制約(NOT NULL、UNIQUE、DEFAULT ...etc)など

Chapter7ではセキュリティとパフォーマンスの最適化を念頭にさらに設計を深化させます。
1. データ整合性を満たすセキュリティ設計
2. データの機密性を考慮した設計
   - 権限設定
     - ユーザーレベル
     - テーブルレベル
     - カラムレベル
   - 暗号化設定
     - パスワードのハッシュ化
     - クレジットカード番号や有効期限などの機密データの暗号化

3. ストレージ効率を意識したテーブルの統一や分割

4. インデックス
    - 標準的なインデックスとしてのB-tree
    - フルテキストインデックス
5. パフォーマンスを意識した非冗長化

Chapter8では、それまでの「The Sci-Fi Collective」とは別に「SHIPS R US」という別の例を用いて、ChatGPT 4oを使用しながら設計を0から行います。
Chapter3~Chapter7で学んだ設計をAIと伴走しながら進めていく内容です。


# 含まれていなかった内容

## 正規化の詳細

本書では正規化については、1NF~BCNFまで正規化が行われたテーブルは〇〇の条件が揃っているという書き方なので、1NFでは何をして...といった詳細はありませんでした。

## トランザクションの話

冗長なテーブル構造の場合に引き起こされる挿入・更新・削除の異常については触れられていたものの、
トランザクションによる制御については触れられていませんでした。

いずれも 『達人に学ぶDB設計徹底指南書 第2版』を併読すれば足りない箇所の補強になるかなと思います。

# 感想

架空のECサイトの例をもとに設計をステップ・バイ・ステップで体験できるとてもいい本だと感じました。
私のようにRDBでのモデリング経験がない者からすると実地でどのように設計をしていくべきかのイメージを持てるのがとてもいいなと思いました。

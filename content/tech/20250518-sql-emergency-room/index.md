---
title: '『SQL緊急救命室 ─⁠─非効率なコードを改善せよ！』を読んだ'
tags: ["database", "rdb", "book review", "データベース設計"] # Lambda, API Gateway, AWS, Python
# description: "" # 個別ページのタイトル下部に表示される
slug: "sql-emergency-room"
summary: "『SQL緊急救命室 ─⁠─非効率なコードを改善せよ！』を読んで、CASE文やウィンドウ関数といったモダンSQLに慣れることができた" # 
cover:
    image: "<image path/url>" # 300 * 180を基準にしたい
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
draft: false
# ----------------
date: 2025-05-18T12:08:01+09:00
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

# さいしょに

ミックさんのデータベース学習ロードマップの4冊目『SQL緊急救命室 ─⁠─非効率なコードを改善せよ！』を読みました。  
実務でありがちな「遅いSQL」「読みにくいSQL」を、モダンな書き方でどう改善できるかを学べる一冊です。

参考：  
https://x.com/copinemickmack/status/1871863629706096770/photo/1

| 書籍情報 | 内容 |
| --- | --- |
| 書籍名 | SQL緊急救命室 ─⁠─非効率なコードを改善せよ！ |
| 著者名 | ミック |
| 出版年 | 2024.9.14 |
| ISBN | 978-4-297-14406-7 |
| 書籍ページ | https://gihyo.jp/book/2024/978-4-297-14405-0 |

# ざっくりと感想

本書は「緊急救命室」というタイトル通り、現場でよく見かけそうな“非効率なSQL”を、より良い形にリファクタリングしていく構成です。  
ロバート、ヘレン、ワイリーという個性的な登場人物たちの会話形式で進むため、専門的な内容でもテンポよく読み進められました。

特に印象的だったのは、  
- サブクエリやUNIONを多用した複雑なクエリを、CASE式やウィンドウ関数でシンプルかつ効率的に書き直すテクニック  
- SQLの標準化の歴史や、アプリケーション設計の観点など、単なる文法解説にとどまらない深い議論  
など、実務に直結する知見が満載だった点です。

# 気になったことのメモ

- **条件分岐はUNIONではなくCASE式を検討**
  複数の条件でデータを分けたいとき、ついUNIONで複数クエリを組み合わせがちですが、CASE式を使うことで1つのクエリにまとめられ、パフォーマンスも向上します。

- **ループ処理したくなったらウィンドウ関数を検討**  
  集計や順位付けなど、従来はアプリケーション側でループ処理していた部分も、ウィンドウ関数を使えばSQLだけでスマートに解決できる場面が増えています。

- **なんでもSQLで解決すればいいってものではない**  
  SQLの力技で無理やり解決するのではなく、そもそもテーブル設計が適切か、集計用のフラグやキーを持たせるべきか、といった設計段階での工夫も重要だと再認識しました。

- **SQLにロジックを寄せるメリット**  
  - SQLには効率化のための機能が豊富に用意されている  
  - 多くの場合、DBサーバーはアプリケーションサーバーよりも高性能で信頼性が高い  
  - ただし、ロジックがSQLとアプリで分散すると、保守性や属人化のリスクもあるため、チーム内でのスキル共有が不可欠  
  - それでも「SQLでロジックを表現できる」という選択肢を持っておくことは大きな武器になる

- **スーパーソルジャーではなくスーパーエンジニアになること**  
  - 与えられた制約の中で最適な選択肢を見極め、実行できることが大切  
  - アクロバティックなテクニックに走るのではなく、事業や現場の目的を見据えた判断力が求められる  
  - 技術だけでなく「どうやって事業を儲けさせるか」という視点も忘れずに持ちたい

# 個人的に仕事で迷いそうなポイント

- 配列型やJSON型など、標準化の過程で議論が分かれた型はもちろん、単純な文字列型ですらベンダーごとに挙動が異なる現状があります。  
  こうした中で「どこまでベンダー間の移植性を意識すべきか？」は悩ましい問題です。

- 一方で、移植性を気にしすぎて便利な機能を使わないのも非効率。  
  「今のプロジェクトにとって最適なのは何か？」を考え、場合によってはロックインを受け入れてでも生産性を優先する判断もアリだと感じました。

# 最後に

本書を通じて、SQLの書き方だけでなく、さまざまな観点で幅広く学ぶことができました。  
「SQLは奥が深い」と改めて実感するとともに、現場で迷ったときの指針になる一冊です。  
SQLに苦手意識がある方や、より効率的なクエリを書きたい方にぜひおすすめしたいです。
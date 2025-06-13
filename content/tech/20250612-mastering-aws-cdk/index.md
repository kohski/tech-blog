---
title: "『マスタリングAWS CDK』を読んで、運用も見据えたCDKの知見を獲得した"
tags: ["AWS", "CDK", "IaC", "インフラ", "書籍レビュー"]
# description: "" # 個別ページのタイトル下部に表示される
slug: "mastering-aws-cdk"
summary: "AWS CDKの実践的な使い方と運用の知見を学んだ書籍レビュー。Lambda、ECS、パラメータ管理、コンストラクト設計など、実務で役立つ内容が充実。"
cover:
  image: "<image path/url>" # 300 * 180を基準にしたい
  alt: "<alt text>" # alt text
  caption: "<text>" # display caption under cover
  relative: false # when using page bundles set this to true
  hidden: true # only hide on current single page
draft: false
# ----------------
date: 2025-06-12T15:37:39+09:00
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
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: false
ShowRssButtonInSectionTermList: true
UseHugoToc: true
---

# はじめに

ちょっと前まで RDB の勉強をしていました。
ちょっとした TODO アプリを作っていて、せっかくだし IaC も CI/CD も整備しようと思っていました。
CDK を久しぶりに触ったのですが（1 年前にやっていた案件では CDK を使っていたが現在は SAM）、全然手に馴染む感じがしなくて、
AI くんへの指示だしもイマイチだし、出してくるコードもこれでいいのか悪いのか判断できないなーという感じがしました。

そこで今一度、CDK に再入門したいな（できれば速習で）って思い、本書を手に取りました。

| 書籍情報   | 内容                                                                                            |
| ---------- | ----------------------------------------------------------------------------------------------- |
| 書籍名     | マスタリング AWS CDK ~現場で実践可能なスキルを身につける~                                       |
| 著者名     | 佐藤智樹                                                                                        |
| 出版年     | 2019.3.6                                                                                        |
| ISBN       | 978-4-297-10409-2                                                                               |
| 書籍ページ | https://techbookfest.org/product/wEWtvT8aRZgzAcVSnE7wS8?productVariantID=9cBbjAi8DX90QNErt4VJnT |
| GitHub     | https://github.com/tomoki10/mastering-aws-cdk                                                   |

そもそも IaC がなぜ必要なのか...という内容から入り、AWS CDK の構成の説明など講義パートが最初にありました。
ここでざっくりツールの理解をした後で、Lambda を中心とした構成、ECS を中心とした構成を実際に作ってデプロイしてみる。
その後、実務を意識した内容(パラメーター管理、スタックやコンストラクトの構成、CI/CD の作成)などを行いました。
特に 7 章〜10 章にかけては、ドキュメントを追っているだけでは気付きづらい内容が多く、CDK を用いて開発・運用を続けてきた筆者の知見が光る内容だなと感じました。

# 印象に残ったこと

## AWS CDK 自体のコンセプトを理解できた

今まで雰囲気で AWS CDK を使っていたので自動で出力されるファイル類を単なるボイラープレート程度にしか理解していなかった。

- App > Stack > Construct というツリー構造をとっていること
- Construct についても実装の抽象度によって、L1, L2, L3 というレイヤー構造で整理されていること

ということを知ることができました。

また、ガイドラインとして、

- Stack は極力分割しない方がいいこと。分割するとしても、Parameter Store を用いて依存関係を作ること
- L3 レイヤーの使用は往々にして避けた方がいいこと

などが提供されていました。
運用を見据えて、目先の楽よりも疎結合であったり簡易であることをとるべきで、それは AWS CDK においてはこういうことっていうことがよくわかりました。

## パラメーター管理において、型安全を実現するってこと

パラーメータ管理についても 5 つのパターンが紹介されており、TypeScript のオブジェクトを使う方法が推奨されていました。

この方法では、型での補完やチェックが効くので、早いフィードバックが得られるというのが推しポイントのようでした。

cdk.json や cli の parameter など複数のコンテキストの提供方法があったのでいろいろと迷っていましたが、明確なガイドラインが示されていて、今後参考にできそうだなと思いました。

## コンストラクトによる整頓

クロススタックの参照は仮に Parameter Store を使用した場合でも、デプロイ順序の制御が必要です。
なので、分割するメリットがない限りは分割しないということがわかった。

しかし、イベントドリブンなアーキテクチャの場合は、処理を追うのに IaC を読み込む必要があったりで、
IaC の可読性ってのは意外と捨ておけない。

そこで Construct によるファイル分割の手法が示されていた。
最初にべたーっと平たく書いたものを分割していくというもので、合間で筆者の解説が入るが、実務を意識した非常に実践的な内容だった。

# 最後に

本書は IaC を実現する 1 ツールである AWS CDK だけの解説にとどまらず、AWS CDK を通して、AWS 上でモダンなアプリケーションをどのように構築・運用していくかが示されていて、とても参考になった。

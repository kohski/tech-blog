---
title: 'APIGatewayからLambdaを非同期で呼び出す'
tags: ["Lambda", "APIGateway", "AWS"] # python, aws, lambdaなどを記述
# description: "" # 個別ページのタイトル下部に表示される
slug: "async-lambda-from-apigateway"
summary: "APIGatewayからLambdaを非同期で呼び出す方法を書きます" # 
cover:
    image: "<image path/url>" # 300 * 180を基準にしたい
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
draft: true
# ----------------
date: 2024-06-17T21:27:32+09:00
author: "kohski"
showToc: true
TocOpen: false
hidemeta: false
comments: false
# canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: false
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: false
ShowRssButtonInSectionTermList: true
UseHugoToc: true
---

## 最初に

APIGateway + Lambdaの構成でアプリケーションを作成していると、APIGatewayの29秒制限というのはしばしば意識するクオータだと思います。
そもそもユーザビリティを考えても29秒でも待たせすぎです。
一方で29秒を超えるユースケースのAPIもしばしば実装が必要になります。
今回は、APIGatewayからLambdaを呼び出す際に非同期で呼び出す方法について調べました。

後にも触れますが、統合リクエストのいてカスタムのヘッダーを追加する必要があります。
その際に、Lambdaプロキシ統合というAPIリクエストの各パラメーターをLambdaのeventにいい感じにマッピングしてくれる設定が使えなくなります。

全体的なイメージを掴みやすくするためにマネコンで手順を残します。

## 事前準備

### サンプルのLambdaを作成


### サンプルのAPIGatewayを作成


## 本論

### 統合リクエストの設定

### 統合レスポンスの設定


## その他

### レスポンスの変更をしたい

## 最後に




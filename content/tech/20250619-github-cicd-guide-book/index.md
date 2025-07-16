---
title: '『GitHub CI/CD実践ガイド』を読んで、'
tags: ["書籍レビュー", "GitHub", "CI/CD"] # Lambda, API Gateway, AWS, Python
# description: "" # 個別ページのタイトル下部に表示される
slug: "github-cicd-guide-book"
summary: "" # 
cover:
    image: "<image path/url>" # 300 * 180を基準にしたい
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
draft: false
# ----------------
date: 2025-06-19T15:20:31+09:00
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

私は普段、業務でGitHub Actionsを利用していますが、ワークフローに手続きをそのまま書き連ねるだけで、機能追加の際も適当にpushトリガーを設定して試行錯誤することが多く、「本当にうまく使えているのだろうか？」という疑問を持っていました。そんな中で、GitHub Actionsのベストプラクティスや設計・運用について体系的に学びたいと思い、『GitHub CI/CD実践ガイド』を手に取りました。

本書は、単なるGitHub Actionsの解説本という枠を超え、現代的なソフトウェア開発の手法をGitHubのサービスを軸に解説しており、非常に満足度の高い内容でした。小さなハンズオンを通じて実践的に学べる構成や、ベストプラクティスの紹介、参考書籍の選定からも著者の見識の深さが伝わってきます。

| 書籍情報 | 内容 |
| --- | --- |
| 書籍名 | GitHub CI/CD実践ガイド ―⁠―持続可能なソフトウェア開発を支えるGitHub Actionsの設計と運用 |
| 著者名 | 野村友規 |
| 出版年 | 2024.5.29 |
| ISBN | 978-4-297-10409-2 |
| 書籍ページ | https://gihyo.jp/book/2024/978-4-297-14173-8 |
| GitHub | https://github.com/tmknom/example-github-cicd |

全体を通して、GitHub Actionsの使い方だけでなく、持続可能なソフトウェア開発を支えるための設計や運用の考え方まで幅広くカバーされており、期待以上の学びが得られました。

# 印象に残ったこと

## 全体的な構成と学び

本書は、GitHub Actionsの基本的な使い方にとどまらず、現代的な開発手法や運用のベストプラクティスまで幅広く解説しています。特に、ymlファイルをただ書くだけでなく、実際の現場で役立つ設計や運用の工夫についても多くの示唆がありました。小さなハンズオンを通じて、実際に手を動かしながら学べる点も印象的でした。

## ECSへのデプロイ

AWSのECSへのデプロイについては、これまでCodeシリーズを使ったCI/CDの例が多かった印象ですが、本書ではGitHub Actionsを活用した実践的な例が紹介されていました。EnvironmentsやSecrets、Variablesを活用した構成情報やシークレット管理の方法は非常に参考になりました。また、段階的に構成を本格化させていくハンズオンの流れも理解しやすかったです。細かいアクションの説明は省略されている部分もあるため、必要に応じて追加学習が必要だと感じました。環境分離についても自分で理解し実装する必要がある点が強調されていました。

## セキュリティ

GitHubにおけるセキュリティの考え方についても詳しく解説されています。特に、リポジトリがセキュリティ境界の主軸であることや、トークンの種類ごとに使うべきもの・避けるべきものが明確にガイドラインとして示されていました。

- 推奨されるトークン：GIITHUB_TOKEN、デプロイキー、GitHub Appsトークン
- 避けるべきトークン：Fine-grainedパーソナルアクセストークン、個人のSSHキー、パーソナルアクセストークン

また、dependabotによる依存関係の管理やセキュリティアップデート、OIDC Providerの活用など、実践的なセキュリティ対策についても触れられています。

## GitHubを用いた開発プラクティス

本書では、GitHubを活用した開発プラクティスが体系的に言語化されている点も印象的でした。例えば、ブランチ保護やオーナーシップの明確化、クレデンシャル検知、READMEやLICENSE、コミュニティヘルスファイルによるドキュメンテーション、リリースやパッケージ管理（ghcrの活用）など、実際の現場で役立つノウハウが詰まっています。

## ワークフローの設計

ワークフローを単にベタ書きするのではなく、actionsやreusable workflow、GitHub Appsを活用して再利用性や保守性を高める方法についても解説されています。これにより、より効率的で持続可能な開発体制を構築できると感じました。

# 最後に

AI時代においては、一つの技術や概念を深く知ることも大切ですが、それ以上に「その存在を知っている」という幅広い知識（いっちょかみ力）が重要だと感じています。本書は、GitHubを活用した開発プラクティスを広範にカバーしており、実践的な知識を得ることができました。

また、AIがコードを書く時代において、人間が担うべき役割の一つはガードレールや開発環境の整備だと考えています。その意味でも、本書は多くの引き出しを与えてくれる一冊でした。GitHub Actionsの解説書という期待を良い意味で裏切る、非常に満足度の高い書籍でした。
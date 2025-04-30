---
title: '『DNSがよくわかる教科書』を読んだ'
tags: ["network", "dns"] # Lambda, API Gateway, AWS, Python
# description: "" # 個別ページのタイトル下部に表示される
slug: "dns-textbook"
summary: "『DNSがよくわかる教科書』を読んで、DNSの仕組みと動作原理がよく理解できました" # 
cover:
    image: "<image path/url>" # 300 * 180を基準にしたい
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
draft: false
# ----------------
date: 2025-04-29T14:09:22+09:00
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

# はじめに

以前の[ネットワーク入門におすすめの2冊を読んで学んだこと](https://kohski.dev/tech/network-entry-book/)のエントリーでも書いたように、苦手意識のあるネットワークの勉強を続けています。特に、実務で登場する機会が多く、苦手意識が強いのがアプリケーション層（L7）で動作するDNSです。

そこで、定評のある『DNSがよくわかる教科書』を読んだので、学んだことや感想をまとめたいと思います。


| 書籍名 | DNSがよくわかる教科書 |
| --- | --- |
| 著者名 | 渡邉結衣、佐藤新太、藤原和典 |
| 出版年 | 2018年11月22日 |
| ISBN | 978-4-7973-9448-1 |
| 書籍ページ | https://www.sbcr.jp/product/4797394481/ |

# これまでのDNSとの関わり

過去の業務でDNS関連では以下のような経験がありました：

- API Gatewayに独自ドメインを設定
- IoT Gatewayに独自ドメインを設定
- CloudFront + S3に独自ドメインを設定

しかし、これらはいずれもドキュメントを読んだり、マネジメントコンソール上の指示に従うだけで、内部の仕組みをきちんと理解していませんでした。

# 読んでみた感想

非常に丁寧に解説されており、大変勉強になりました。
この本の良いところは、単に名前解決ができればいい初心者から、権威サーバーやフルリゾルバーを運用するような上級者まで、幅広いニーズをカバーしている点です。

## DNSの登場人物の整理ができた

- **hostsファイル**
    - 以前の名前解決の主要メカニズム
    - 現在でも使われている
- **スタブリゾルバー**
    - PCやスマホに組み込まれているソフトウェア
    - アプリケーションからの要求を受けて動作
    - フルリゾルバー（間に介在するならフォワーダー）に対して**名前解決要求（再起問い合わせ）**を行う
- **フォワーダー**
    - フルリゾルバーに名前解決要求を転送
    - 名前解決の結果をキャッシュする
    - 自宅のルーターやサーバーなどに搭載されていることが多い
- **フルリゾルバー**
    - 名前解決のための反復問い合わせを実行
        - ルートサーバーから順に権威サーバーに対して問い合わせる
        - CNAMEレコードを受け取った場合は、再度ルートサーバーから反復問い合わせをする
    - 名前解決結果のキャッシュを管理
- **権威サーバー**
    - ゾーン情報を管理
    - 問い合わせに対して応答

## トラブルシューティング方法が理解できた

- **有用なコマンドラインツール**
    - `dig`
    - `drill`
    - `kdig`
    - `nslookup`
    - など
    - Windowsの一部環境では`nslookup`が使われていますが、`dig`の方が機能が豊富で推奨されています

- **実践的なトラブルシューティング**
    - 自分がフルリゾルバーになったつもりで、反復問い合わせのみを使って名前解決を体験できました
        - Aレコードが直接返ってくる場合
        - CNAMEが設定されている場合
            - これについては複雑なので、別記事で詳しく解説する予定です

## その他学んだこと

- **DNSにまつわる攻撃手法**
    - DNSリフレクター攻撃
    - ランダムサブドメイン攻撃
    - キャッシュポイズニング
    
- **よくあるトラブルとその解決方法**
    - lame delegation（不適切な委任）
    - TTL設定の問題と影響

この他にも数多くの項目について詳しく解説されていました。

## 記載されていなかった最新技術

本書は2018年出版のため、HTTP/3関連のHTTPSレコード（HTTPS RR）についての記載はありませんでした。これは仕方のない部分だと思います。最新のDNS技術については、別途情報を補完する必要があるでしょう。

# まとめ

この本を読むことで、特に以下の点が明確になり、DNSへの苦手意識が大幅に軽減されました：

- DNSに関わる各コンポーネントの役割が整理できた
- 各コンポーネントがキャッシュを持っている仕組みが理解できた
- 実際のトラブルシューティング方法が具体的に学べた

今後はDNS関連のタスクにも自信を持って取り組んでいきたいと思います。
次回は、CNAMEを含む名前解決の流れについて、具体例を交えて詳しく解説する予定です。

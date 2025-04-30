---
title: 'フルリゾルバーの気持ちでCDNを前段においたドメインの名前解決をたどる'
tags: ["network", "dns", "dig", "cdn"] # タグを追加
# description: "" # 個別ページのタイトル下部に表示される
slug: "dns-full-resolver"
summary: "フルリゾルバーになり切ってCDN配置されたドメインの名前解決プロセスを追跡した記録" # サマリーをより具体的に
cover:
    image: "<image path/url>" # 300 * 180を基準にしたい
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
draft: false
# ----------------
date: 2025-04-30T10:08:55+09:00
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

『DNSがよくわかる教科書』のChapter08「digコマンドの応用〜フルリゾルバーになって名前解決〜」の例2が、書籍の例示どおりには動作しなくなっていた。
そこで、`www.nike.com` を対象に同様の演習内容を実施してみた。
具体的には、`dig` コマンドを使い、再帰問い合わせ（`+rec`）を禁止し、**反復問い合わせ（`+norecurse`）のみ**で名前解決プロセスをたどった。

# DNSの基礎知識

## DNSの主要コンポーネント

| 名前 | 役割 |
|:--|:--|
| スタブリゾルバー | クライアント（PCやスマホ）に組み込まれた簡易DNS問い合わせ機能。名前解決要求を行うだけ |
| フォワーダー | 社内ネットワークなどに配置される中継専用DNSサーバ。自身で名前解決せず、フルリゾルバーにリクエストを中継する |
| フルリゾルバー | ルートサーバからTLD、権威サーバまで自ら問い合わせて答えを得るDNSサーバ |
| 権威サーバ | 特定のドメイン名について正式な情報（Aレコードなど）を保持・管理するサーバ |

## 再帰問い合わせと反復問い合わせの違い

- **再帰問い合わせ（Recursive Query）**
  - 「最終的な答えが得られるまで再帰的に問い合わせてください」とリゾルバーに依頼する方式
  - クライアントは1回のリクエストで最終的な結果を得られる
  
- **反復問い合わせ（Iterative Query）**
  - 「あなたが知っている範囲の情報だけ教えてください」という問い合わせ方式
  - 知らない情報については「次に尋ねるべきサーバ」の情報が返される
  - 自分で段階的に問い合わせを繰り返す必要がある

今回の実験では**反復問い合わせのみ**を使用し、フルリゾルバーの動作を手動で再現した。


## 実施したコマンド群と解決過程

### 1. ルートサーバから `.com` のNSレコードを取得

```bash
dig +norec @a.root-servers.net com NS
```

### 2. `.com.`のTLDサーバから `nike.com.` のNSレコードを取得

```bash
dig +norec @l.gtld-servers.net nike.com. NS
```

### 3. Nikeの権威サーバから `www.nike.com.` のAレコードを取得

```bash
dig +norec @205.251.197.8 www.nike.com. A
```
→ 結果: CNAMEレコード `www-geo.nike.com.akadns.net` を取得


### 4. `.net` のNSレコードをルートサーバから取得

```bash
dig +norec @a.root-servers.net net NS
```

### 5. `.net.`のTLDサーバから `akadns.net.` のNSレコードを取得

```bash
dig +norec @192.55.83.30 akadns.net. NS
```

### 6. `akadns.net.`の権威サーバから `www-geo.nike.com.akadns.net.` を問い合わせ

```bash
dig +norec @96.7.49.129 www-geo.nike.com.akadns.net. A
```
→ 結果: 新たなCNAME `www.nike.com.akadns.net` を取得


### 7. `www.nike.com.akadns.net.` をさらに問い合わせ

```bash
dig +norec @96.7.49.129 www.nike.com.akadns.net. A
```
→ 結果: 次のCNAME `ev-cn.nike.com.edgekey.net` を取得


### 8. `edgekey.net.` ドメインの情報を取得するため、`.net.` → `edgekey.net.` をたどる

```bash
dig +norec @a.root-servers.net net NS
dig +norec @192.55.83.30 edgekey.net. NS
```

### 9. `edgekey.net.` の権威サーバに `ev-cn.nike.com.edgekey.net.` を問い合わせ

```bash
dig +norec @23.211.133.65 ev-cn.nike.com.edgekey.net. A
```
→ 結果: さらに長いCNAME `ev-cn.nike.com.edgekey.net.globalredir.akadns.net` を取得

### 10. `akadns.net.` の権威サーバに再度問い合わせ

```bash
dig +norec @192.55.83.30 akadns.net. NS
dig +norec @96.7.49.129 ev-cn.nike.com.edgekey.net.globalredir.akadns.net. A
```
→ 結果: 別のCDNドメイン `e2785.x.akamaiedge.net` へのCNAMEを取得


### 11. `akamaiedge.net.` のNSレコードを取得

```bash
dig +norec @192.55.83.30 akamaiedge.net. NS
```

### 12. `e2785.x.akamaiedge.net.` について問い合わせ

```bash
dig +norec @184.26.161.192 e2785.x.akamaiedge.net. A
```
→ 結果: Akamaiの複数NSサーバ（n0x〜n7x）が返された


### 13. 最終的に特定のNSサーバに問い合わせてAレコードを取得

```bash
dig +norec @88.221.81.192 e2785.x.akamaiedge.net. A
```

最終的に得られたIPアドレス

```
23.36.16.98
```

## 学んだこと

- DNSは右から左へラベル単位で階層をたどって解決される
- CNAMEレコードに遭遇した場合、そのホスト名を再度名前解決する必要がある
- ゾーンの境界をまたぐ際は、新たな権威サーバへの問い合わせが必要になる
- `.com`と`.net`のようなTLDをまたぐ場合は、再びルートサーバから辿り直す
- CDN（今回はAkamai）は多段CNAMEを採用している

## 感想

名前解決を自分の手で一歩ずつ追跡することで、日常何気なく使用しているDNSの背後で動作している複雑なプロセスを実感できた。
また、Akamaiのようなグローバルなコンテンツ配信ネットワークが多段でCNAMEを設定することで高度なネットワーキングを実現していることが垣間みえた。
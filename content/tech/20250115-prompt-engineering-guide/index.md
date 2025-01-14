---
title: 'Prompt Engineering Guideを読んだ'
tags: ["untagged"] # Lambda, API Gateway, AWS, Python
# description: "" # 個別ページのタイトル下部に表示される
slug: "read-prompt-engineering-guide" #
summary: "Prompt Engineering Guideを読んだ。やっと入門できたかもしれない。" # 
cover:
    image: "<image path/url>" # 300 * 180を基準にしたい
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
draft: false
# ----------------
date: 2025-01-15T05:58:22+09:00
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


[『コード×AIーソフトウェア開発者のための生成AI実践入門』を読んだブログ](https://kohski.dev/tech/code-by-generative-ai/)を書いたが、その中でPrompt Engineering Guideを読んで見るといいよとあったのでざっと読んでみた。

筆者は、元々非エンジニア職をしていて、ITエンジニアになったのは5年くらい前。AWS上でサーバーレスなものを作ることにずっと従事してきました。
AI/MLは初心者です（AWS資格勉強の一環でMLSを取得したことはあるが、基本的には全暗記で内容は理解していない）

ちゃんとAIを乗りこなさないとおまんまの食い上げになってしまうので、読んだ本のアドバイスには従っていこうということで今回掲題の通りとなりました。


# 読んだ内容をかいつまんでみる

こういったドキュメントをローラーでざーっと読んでも全てが自分の実業務に活きるわけではありません。
私の個人の問題意識と合致した部分をメモしていきます

## 英語で読もう

あらゆるドキュメントがそうですが、日本語はマイナー言語なので翻訳が追いついていない部分が多いです。
それどころか、Prompt Engineering Guideの日本語版においては、目次すらも最新版になっていません。

ここは他のリソースのドキュメントと同じく極力英語で読むことをおすすめします。
とはいえ、[Introduction](https://www.promptingguide.ai/introduction)や[Prompting Techniques](https://www.promptingguide.ai/techniques)などの動きの少ないページは日本語でも最新版になっていそうなので、基礎をざっと眺めたいという用途には日本語でも十分だと思います。


## 基本をつかもう

ChatGPTが誕生して依頼、チャット形式・自然言語でのやりとりができるため、筆者は特に基本的なものは学ばずに試行錯誤でプロンプトを書いてきました。
コードを書く仕事という性質上、ワンショットのプロンプトが書ければいいので今までは十分でしたが、やはり効率的なプロンプトの書き方は存在するようです。

以下の章を読むことでPrompt Engineeringの基本をつかむことができました。

- [Introduction](https://www.promptingguide.ai/introduction)
- [Prompt Techniques](https://www.promptingguide.ai/techniques)
- [Guides](https://www.promptingguide.ai/guides/optimizing-prompts)

また、効果的なプロンプト例は以下にまとまっています。

- [Prompt Hub](https://www.promptingguide.ai/prompts)


## セキュリティの記述は有用

生成AIツールは毎日のように新しいものが生まれ、アーリーアダプターな開発者たちが多くの発信をしています。
ただ、そのどれもが「Todoアプリ爆速でできた！」「アイデアを早く形にしてマネタイズ！」みたいなキラキラしたものばかりです。
筆者としては、プロダクト利用での知見みたいなのがもっと知りたいなーなんて思うわけです。

そんな中でセキュリティに関する記述があるのが非常に良かったです。
特に敵対的プロンプティングについて詳しいのでこの辺は必読だなーと思いました。

- [Adversarial Prompting](https://www.promptingguide.ai/risks/adversarial)


## 昨今話題のAgentについては頻繁に更新されている

「AIエージェント元年」などと昨今喧伝されているようにAgentの技術は注目されている分野です。
「そもそもエージェントってなんや？」レベルの筆者からすると、基礎的な内容に触れられている以下の章は勉強になりました。

- [Agent](https://www.promptingguide.ai/agents)

ここの章は現在も頻繁に更新されているわりに、日本語版は項目すら目次に出てきていないです。

## 注意点

古い情報なのか、最新の情報なのか分かりづらいというのが、Prompt Engineering Guideのやや難しい点だなと思います。
Few-shot Promptの例なんかは、最新のモデルではZero-shotで回答してくれたりとかっていうのもあります。
なので、GitHubで該当ページの更新日時を確認するなどして、情報の新鮮度を確認して、採用の可否を決めるのが良さそうです（だいぶ手間ですが）


# 最後に

Prompt Engineering Guideを読んでみました。
有志による更新・翻訳なので、一部不便な点はありますが、Prompt Engineeringの基礎を学べる良い資料だなと思いました。





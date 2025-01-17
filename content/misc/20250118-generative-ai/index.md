---
title: '同僚と生成AIについて話した'
tags: ["untagged"] # Lambda, API Gateway, AWS, Python
# description: "" # 個別ページのタイトル下部に表示される
slug: "talk-about-generative-ai" #
summary: "生成AIの話ばかりね、最近" # 
cover:
    image: "<image path/url>" # 300 * 180を基準にしたい
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
draft: false
# ----------------
date: 2025-01-18T06:25:03+09:00
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

最近会社の同僚と生成AIについて話す機会が多くて、結構面白かったのでメモ

なお、ここでいう生成AIは主にテキストコミュニケーションをするものを指しており、動画や画像の生成は範囲外。用途は主にプログラミングコードの生成。

# すでに使い方に結構差がある

私の部でも凄腕とされている同僚は本人がNeovim使いということもあり、あまりGitHub Copilotを使っていないようだ。（GitHub Copilotってvscode重視だしね）
じゃあ、コーディングにどうやって生成AIを活かしているかというと、
https://github.com/mufeedvh/code2prompt といったツールを使って、抽出したコードの情報を丸ごとチャット型のインターフェースに流し込んでいるらしい。

対する私はGitHub Copilotがんがん使っている。vscode派だし。
CompletionもChatも使う。Editはまだあんまり。
Chatで容量を得ないときは、チャットインターフェースでo1 miniとか使う。

要は同僚との差分は、Completionを使うかどうかである。
これ、多分同僚くらい手が早く動く & 完成イメージが鮮明だとCompletionって邪魔っぽいんだよね。

エンジニアとしての質や身につけてる技術によって、必要とする道具も違うよねっていう発見でした（書いてみると当然ね。それ）


# 教育がぐっとかわるよねー

「最近の若年層では生成AIを使用して学力の向上に繋げる人が多く、大学入試に地殻変動が起きつつある」
みたいなのをXで見た。（該当ポストを見つけられない）

従来はストレージ型頭脳のクソ暗記で東大まで行けたけど（文系は特に。理系は違うかも）、ちょっと違う時代が来てそうだなーと思った。
問題の正解への到達のコストがぐっとさがるので、違いを生むのは好奇心と行動力ってことになりそう。

そこで、我が子への接し方として、
- 一緒にいろんなことを面白がって好奇心を伸ばす
- 若いうちから生成AIへのアクセスを確保する

なんてことを考えていたのよ。

でも、同僚たちと話してて、何が真実かわからない段階で生成AIをわたしちゃうのもねーみたいな話があって、それもそうねーって思ったわけです。
むずいね、教育。

# o1 proいうほどか？

同僚複数名がそう言ってた。
ワイは貧乏なので、o1 proは触ってない。

X上では、o1 proがあれば〇〇がいらないってよく言ってます。
そういう人たちと比較して、対処する問題の質とその向き合い方がエンジニアって独特なのかなーと思ったりする。

まず、われわれ開発エンジニアに仕事が来る時って、だいたいやりたいことのイメージが定まってると思う（それが妥当か、誰がそのイメージ持ってるか...などは置いといて）。
それを具体化していって、より洗練させて、さらに具体化して...てのが我々の仕事だと思う。
いわば収斂の仕事なんだと思う。

また、あらゆる仕事って、サブタスクに分割すると思う。
その分割したサブタスクが十分に具体的なのがエンジニアのタスクの質なのかなーと思ったりする。
そうなってくると、o1 proの創造性ってあんまり必要じゃなくて、そんなことより演算時間の長さやコストの方が目についちゃうのかもな。

まあ、この辺の仕事の質も変容を求められるだろうし、サブタスクの分割もAIにやってもらおうってことにはなるのかもなー。
現状では、o1 proわいはまだ様子見で行くわーって気持ちである（50USD程度まで落ちてくれればなー）
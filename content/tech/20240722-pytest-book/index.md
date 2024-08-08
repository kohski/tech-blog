---
title: 'テスト駆動Python[第2版]を読んだ'
tags: ["untagged"] # Lambda, API Gateway, AWS, Python
# description: "" # 個別ページのタイトル下部に表示される
slug: "test-driven-python"
summary: "テスト駆動Python第2版を読んだ" # 
cover:
    image: "<image path/url>" # 300 * 180を基準にしたい
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
draft: false
# ----------------
date: 2024-07-22T22:00:43+09:00
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

# きっかけ

- Pythonを使用しているプロジェクトに新たにジョインすることになった
- キャッチアップにあたってはテストのコードリーディングが一番近道
- いち早くなれるために、ハンズオン形式で進めやすい本書を手に取った。
    - Pytestについては日本語の公式ドキュメントもあるため、そちらで十分という人もいると思う
    - ドキュメントだとテスト対象のコードがシンプルすぎる場合もある。
    - 実ユースケースに近いのかなと思って選んだ
- 目的にそぐわない部分は斜め読みしかしていない

# おすすめの方

- Pythonについての基礎的な学習が済んでいる方
- Pytestを速習したい方

# ちょっとイメージと違った点

- 『テスト駆動Python』という名称から、PythonでTDDを実施することを想定する人もいるかもしれないが、本書は違う。
- テスト対象のコードは用意されており、テストを書くだけである。

# 書籍の紹介

- 『テスト駆動Python』では、`cards`という架空のCLIアプリケーションを対象にして、テストを書きながら、Pytestの基本を学んでいく
    - サンプルプロジェクトは[The Pragmatic Bookshelf](https://media.pragprog.com/titles/bopytest2/code/bopytest2-code.zip?_gl=1*fk910k*_ga*MTM0NDI3ODYxMS4xNzE5MTI5MjIz*_ga_MJ4659LJZC*MTcyMTY1NDIzMi42LjEuMTcyMTY1NDI0Ni4wLjAuMA..)からDLする
- 章末に復習コンテンツや練習問題が用意されており、理解度を測るのに役立つ。
    - 難易度は書籍内容の範囲内。発展的な内容ではないので特に困らない。
    - 詳しい解説はない

## 概要

## 第1章 初めてのpytest
- pythonの仮想環境
- pytestのinstall
- テストディスカバリ
- テスト結果の確認

## 第2章 テスト関数を書く
- サンプルプロジェクトを題材に早速テストを書く
- アサーション
  - 一般的なアサーション
  - 例外
  - ヘルパー関数化
- 構造化
  - Given/When/Then
  - Arrange/Act/Assert
  - [感想] 個人的には、AAAパターンはどれがどれだかわかりづらいのでGWTが好き
  - [感想] 特に単体テストのような単純なテストケースにおいては、Whenは自明。その前がGivenでその後がThenなので、実はこの辺のコメントいらないのではと思っている。
  - [感想] テスト対象アプリケーションの都合で、E2Eで複数操作が入る場合などは、コメントしといた方がいいとは思う。
- テストの部分実行
  - marker
  - テストコマンド
  - [感想] pytestの-vvオプションはかなりのログ出力量。なので、テストの部分実行をパターンを押さえておくのは非常に大事と思う


## 第3章 Fixture
- Fixtureの話
- デコレーターをつけて、テストメソッドの引数に渡す方式
- `--setup-show`でfixtureの実行順序を見ることができる
    - [感想] プロジェクトに途中から入る場合はこれが必須
- Fixtureのスコープ
- conftest
- `--fixtures`オプションでfixtureの定義場所が分かる
    - [感想] これも大事
- Fixtureの動的スコープ
- autouse


## 第4章 組み込みFixture
- 組み込みFixture
    - tmp_path, tmp_path_factory
    - capsys
    - monkeypatch
- monkeypatchは

## 第5章 パラメーター化
- `@pytest.mark.parametrize`を用いたテスト関数のパラメーター化
- Fixtureのパラメーター化
    - テストごとに実行するセットアップやティアダウンに必要
- [感想] pytest_generate_testsはよくわらかなかった
- `-k`オプションでテスト対象を絞る
    - [感想] `@pytest.mark.parametrize`で与えた引数がテスト名にプリントされるので、それらを絞り込むのに便利

## 第6章 マーカー

- よく使うもの
    - @pytest.mark.parametrize() 
    - @pytest.mark.skip()
        - 将来対応機能でテストだけ書いとくみたいなパターンで使うらしい
    - @pytest.mark.skipif()
        - parameterizeしている中で、特定バージョンをスキップみたいなので使う
    - @pytest.mark.xfail()
        - 失敗前提のテスト
        - 将来対応前提のテストを書いておく、対応できたら外していく
- カスタムマーカー
    - smokeなどの使い方
    - 野放図にならないようにpytest.iniで使用可能なマーカーを入れておく
    - テストの部分実行に便利
        - 実行時間の長いテストで、PRの都度流したいハッピーケースと、たまにでいいエラーケースなどの使い分けを明示的にできる。
        - 多分jestにはない。jestの場合は、
            - テスト名で工夫してテスト実行コマンドを工夫
            - ファイルごと分けるなどかな...
            - エディターの支援を受けづらい


## 第7章 戦略
- テストすべき対象は何かをcardsプロジェクトを参考にしていく
- テスト対象コードのアーキテクチャとテスト観点
- テスト戦略は書き留めることが大切


## 第8章 設定ファイル
- 以下のあたりの説明
    - pytest.ini等の書き方
    - 以下のはいい感じ
        - addopts = 自動で適用するフラグ
        - testpaths = テスト対象のディレクトリ
            指定しないと延々とテストディスカバリが走るっぽい
        - markers = 先述


## 第9章 カバレッジ
- 設定方法
- 結果の確認方法
- カバレッジ追いすぎるなって話

## 第10章 モック
## 第11章
## 第12章
## 第13章
## 第14章
## 第15章
## 第16章

## 感想

- 今までPytest, Jest, Mocha, Rspecなどのテストフレームワークを扱ってきた。
    - assertionやdescribe・it・test等の表現はだいたい一緒。すぐなれる。
    - セットアップやティアダウン, mockには特色が出る
- 
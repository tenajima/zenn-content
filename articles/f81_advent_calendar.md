---
title: "QiitaのタグとWord2Vecで楽しくなる冬"
emoji: "🎅"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Python", "word2vec", "networkx"]
published: false
---

こちらは[Fringe81 Advent Calendar](https://qiita.com/advent-calendar/2020/fringe81)19日目の記事です。
18日目を担当した[trapiの記事](https://zenn.dev/trapi/articles/dataflow_python)からのバトンをつなぎます!

# 概要

- 「機械学習」という言葉に対して関連するワードを返すようなものを作りたいというモチベーションです
- 情報源としてQiitaのタグの情報を使わせてもらいました
- 記事タイトルが迷走してしまったことをこの場を借りてお詫び申し上げます

# できたもの

- 「機械学習」というワードに関連するものとして得られたものが以下の図です
- フムフム、なんとなく良さそうな肌感を得られますね

![機械学習の関連ワード](https://storage.googleapis.com/zenn-user-upload/3fcqeco2aapd49r15vi8nhbaqr4j)

サンプルコードはこちらにアップロードしてありますので理解の助けにお使いください
https://github.com/tenajima/f81_advent_calendar_2020

# 手法

大枠としては
1. Qiitaからタグ情報の取得
2. 無向グラフの作成
3. ランダムウォークさせることによるシーケンスの生成
4. Word2Vecの学習

## 1. Qiitaからタグ情報の取得

タグと言ってるのはQiitaのこの部分のことです。
![Qiitaのタグ](https://storage.googleapis.com/zenn-user-upload/36agdub6fwynkfssk5modj7b1w42)

- このタグ情報を[Qiita API](https://qiita.com/api/v2/docs)を用いて取得します
  - 今回は最新の記事(2020年12月某日時点)1万記事分のタグ情報を用いました

## 2. 無向グラフの作成

- 同一記事につけられているタグは関連しているとして、タグ同士を無向グラフとして結びつけます
  - 上の画像のタグだと「Python - VSCode」「Python - PyConJP」「VSCode - PyConJP」のように結びつけられます

## 3. ランダムウォークさせることによるシーケンスの生成

![ランダムウォークのイメージ](https://cdn-ak.f.st-hatena.com/images/fotolife/n/netres/20180706/20180706030104.png)
- 出来上がったグラフ上でランダムウォークさせます
- 上記の記事以外に「Python - 機械学習」という結びつきを持つ記事が他にあったら「Python」を通じて「機械学習」と「PyConJP」に関連性が見えてくることになります
  - DeepWalkに関しては[こちらの記事](https://netres-bigdata.hatenablog.com/entry/2018/07/06/042240)が分かりやすくまとまっていると思います

## 4. Word2Vecの学習

- 出来上がったシーケンスをWord2Vecに与えて学習させます
- Word2Vecの雑なイメージとしては
  - 「VSCode」という言葉の周りによく「Python」という言葉がある、同様に「VSCode」という言葉のまわりに「R」という言葉もよく出てくるな、「Python」と「R」は似たような意味があるのでは?というような学習です
- そんな感じでタグの意味的な情報を学習させたいというモチベーションです


# 出来上がったもので遊んでみる

## 意味的に近いものを眺める

- 弊社`elm`好きに向けて`elm`に近いワードを取得してみる
- いかがですかね?
- IEとかはつらみが発生してるとかなんですかね?
![elmの結果](https://storage.googleapis.com/zenn-user-upload/ouukkx3f4t63zjp2w85omqpf3i06)

- 弊社`scala`好きに向けて`scala`に近いワードも取得してみます
- いかがですかね?
- それっぽいライブラリの名前がある気がします
![scalaの結果](https://storage.googleapis.com/zenn-user-upload/pomx9uye45rea0m5g9no3ycuztrd)

## Word2Vecならではの遊びで遊んで見る

- Word2Vecといえば「王様」 - 「男」 + 「女」 = 「女王」というような演算ができるということで有名ですのでそれをやってみます

まずは`elm`が出ることを想定して「frontend」 + 「functionalprogramming」を試してみます。
![elmが出て欲しい!](https://storage.googleapis.com/zenn-user-upload/sfwino3vqgf40icdslkkhtphrf7b)

オッ!一番上ではないもののelmが確認されますね!

次は「python」 + 「機械学習」を試してみます
![何が出るかなー](https://storage.googleapis.com/zenn-user-upload/im5abgpo0nhz54f185mqf6wjkp27)
機械学習に強く引っ張られている気がしますが、`sklearn`が出ているのは個人的に嬉しいです。

この演算は「何が出るかなー」と遊んでみるのは楽しいですが、どんな場面で使えるかわ現状浮かんでいません...

# (おまけ)最後に最後に中心性を確認してみる

せっかくなので最後に固有ベクトル中心性を確認してみます。なんのワードがタグとしては中心に位置しているかとざっくり理解してください。
![中心性](https://storage.googleapis.com/zenn-user-upload/a3ihsco0s4iqgroaj0p1jgabkuo8)
- ほぼQiitaのタグ・ランキングと一致します
- 差分はタグ・ランキングでは`swift`が入っていましたが、中心性では`node.js`がランクインしていて、`node.js`のほうがネットワーク的な重要性が高い(他のいろんなタグと結びつくなど)と考えられます

# まとめ・考察

- 関連するワードを得るモデルとしてはそこそこ肌感のあるものがえられました
- ランダムウォークする際に同じ確率でランダムウォークさせましたが、ここにここに重みをつけると結果が変わりそうだと思います
- それよりもQiitaのタグにはtypoも結構含まれているのでデータクレンジングも効きそうだなと感じました
- いろんな問題をネットワークに落とし込むという手は今後も使えそうだなと感じました
- 作っていて楽しかったので満足です

よし、バトンはつないだ!
明日は期待の新星[ガル](https://qiita.com/gal1996)です!楽しみ!

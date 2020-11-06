---
title: "colabでcuDFを使いたかったらここを参照しろ！"
emoji: "📍"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [Python,colab]
published: false
---

:::message alert
kaggle notebookでも試してみましたが、エラーは出ないものの、30分経過してもインストールが終わらず、断念しました。
あくまでcolabでcuDFを使うときの参考にしてください。
:::

:::message
画像及び動作確認は2020年11月20日時点におけるものになります。
:::

# 概要

- cuDFをcolabで使ってみたいけど、インストールのハードルが高そう...と思ってる方向け
- 「cuDF colab」でググるといろんなcolabのサンプルが出てくるけどなんかどれ試してもうまく行かないな...という方向け
- この記事でもメンテできないようなコードは載せません、目的地(colabでcuDFを使えてる状態)に達するための案内標識であろうと思います

# 結論

[RAPIDS公式ページ](https://rapids.ai/start.html)のnotebookを参考にしましょう。

![RAPIDSのページ](https://storage.googleapis.com/zenn-user-upload/t1k5swcmp4nsy2ggmiwoik9qi5dw)
*2020年11月6日時点のページとcolabのリンクの位置*

下記の部分がGPUの確認とcuDFのインストール部分です。
![RAPIDS公式のnotebook](https://storage.googleapis.com/zenn-user-upload/9dwwzr6tqtaqdvjwadu1wn3qobif)
*2020年11月6日時点におけるcuDFのインストール部分*

- 大きいテーブルデータの処理には、感動するほど計算時間短縮になるのでぜひ一度体感してみてください。

# 補足

- 冒頭の注意書きにも書きましたがkaggle notebookではインストールが終わらなかったです
- 状況としてはsolving environmentでずっと止まっている状態でした。
- colabだとT4、kaggle notebookだとP100使ってる差異が影響しているのだろうか...

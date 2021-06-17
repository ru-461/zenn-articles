---
title: "Vue で技術系アイコンを使いたい！ [DEVICON]"
emoji: "🤔"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["npm", "vue", "初心者", "devicon"]
published: true
---

# Vue.js で DEVICON のアイコンを使いたい

[DEVICON](https://devicon.dev/)は技術系のアイコンをアイコンフォントとして配布しているサイトでプログラミング言語のロゴなどが豊富に用意されています。以前投稿した [こちらの記事](https://zenn.dev/ryuu/articles/8f7513d83f05c77d06a3)でも使用しました。

今回は Vue CLI を使い作成したサイト内でアイコンを使おうとした際に少しハマったのでアイコンの導入方法をメモとして残しておきます。

# npm でインストールできそう

![Devicon v2 npmページの画像](https://storage.googleapis.com/zenn-user-upload/xsxld7fv2za6nbhn4g0wqae9oaej)
*説明を見たところ npm でインストールして使えるみたい*

# 説明のとおりにインストールしてみる

ターミナルから以下のコマンドでインストール。

```shell
npm i devicon
```

アイコンを表示させるためテンプレート内に以下のタグを入れてみる。

```html:hello.vue
<i class="devicon-vuejs-plain"></i>
```

しかし、これだけでは表示されませんでした。

# どうやって使えばいいの？

npm のページを翻訳したりしてみたり、Google で使い方について検索してみましたが、解決策を書いてあるページは見当たらなくてハマりました。初めて npm を使って開発したので知識不足な部分もあるのかなと `index.js` や `App.vue` ファイルの中で試行錯誤した結果、時間はかかりましたが導入に成功しました。

# 解決策

App.vue で以下のように記述。

```js:App.vue
import "devicon";
```

![Vue.jsロゴアイコンの表示に成功した様子](https://storage.googleapis.com/zenn-user-upload/89tf7egm2ucsgktxunx1sza6n2op)

`App.vue` の中に上の一文を追加したところ、うまく表示してくれました。
アイコンフォントを npm で使ったことがなかったためかなりの時間ハマってしまいました。他の CSS ファイルと読み込み方が異なったので少しわかりにくいと感じました。
このあと `index.html ` の中にリンクタグを直接記述し CDN から読み込むのも試したところできました。今回導入するにあたって npm と vue の理解がまだまだ足りないなと感じました。npm を使うならば使うべきではない言葉なので修正してくださいなテクニックのため情報がなかったとかでしょうか。もっと掘り下げて調べてみる必要がありそうです。npm で DEVICON のアイコンを導入してる記事や情報がなかったので備忘録として残しておきます。同じようにハマっている人の参考になれば幸いです。

最後まで読んでいただきありがとうございました。

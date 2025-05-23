---
title: "Vueで技術系アイコンを使いたい！【DEVICON】"
emoji: "🤔"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["npm", "vue", "初心者", "devicon"]
published: true
---

## Vue.jsでDEVICONのアイコンを使いたい

[DEVICON](https://devicon.dev)は技術系のアイコンをアイコンフォントとして配布しているサイトでプログラミング言語のロゴアイコンなどが豊富に用意されています。以前投稿した[こちらの記事](https://zenn.dev/ryuu/articles/8f7513d83f05c77d06a3)でも使用しました。

今回はVue CLIを使い作成したサイト内でアイコンを使おうとした際に少しハマったのでアイコンの導入方法をメモとして残しておきます。

## npmでインストールできそう

![Devicon v2 npmページの画像](/images/how-use-devicon/image01.png)
*説明を見たところnpmでインストールして使えるみたい*

## 説明のとおりにインストールしてみる

ターミナルから以下のコマンドでインストール。

```shell
$ npm install devicon
```

アイコンを表示させるためテンプレート内に以下のタグを入れてみます。

```html:hello.vue
<i class="devicon-vuejs-plain"></i>
```

しかし、これだけでは表示されませんでした。

## 解決策

App.vueで以下のように記述。

```js:App.vue
import "devicon";
```

![Vue.jsのロゴアイコン表示に成功した様子](/images/how-use-devicon/image02.png)

App.vueの中に上の一文を追加したところ、うまく表示してくれました。

## おわりに

アイコンフォントをnpm経由でインストールしたことがなかったためかなりの時間ハマってしまいました。他のCSSファイルと読み込み方が異なったので少しわかりにくいと感じました。
このあとindex.htmlの中に`<link>`を直接記述しCDNから読み込むのも試したところできました。

今回導入するにあたってnpmとvueの理解がまだまだ足りないなと感じました。npmでDEVICONのアイコンを導入してる記事や情報がなかったので備忘録として残しておきます。参考になれば幸いです。

最後まで読んでいただきありがとうございました。

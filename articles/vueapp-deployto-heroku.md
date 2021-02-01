---
title: "Vueで作成したSPAをHerokuでデプロイするまで"
emoji: "📡"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["vuejs", "heroku", "wsl2"]
published: false
---

# はじめに

Vue で作成した SPA をデプロイする際には [Netlify](https://www.netlify.com/) や Github Pages といったホスティングサービスを使う方法があります。その中で今回は [Heroku](https://jp.heroku.com/) にてデプロイする方法を調べてみました。
以前、[こちら](https://zenn.dev/ryuu/articles/try-vuetifyapp)の記事で VueCLI を使って開発した SPA を実際にデプロイしていきます。

以前 Heroku にアプリをデプロイした経験はありましたが、VueSPA をデプロイした経験がなかったため、今回は Heroku にデプロイし動かせるところまでの流れについてまとめてみます。

# 環境

- Windows10 バージョン 20H2
- WSL2 (Ubuntu 20.04.1 LTS (Focal Fossa))
- Node.js v12.18.4
- VueCLI 4.5.10
- herokuCLI

# 現状

- 以前 Vagrant を使って Heoku に PHP 製アプリをデプロイした経験はあり
- WSL2 で Heroku を使ったことはない
- VueCLI で作った SPA を Heroku にデプロイできるのかわからない


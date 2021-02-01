---
title: "Vueで作成したSPAをHerokuでデプロイするまで"
emoji: "📡"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["vuejs", "heroku"]
published: false
---

# はじめに

Vue で作成した SPA をデプロイする際には [Netlify](https://www.netlify.com/) や Github Pages といったホスティングサービスを使う方法があります。その中で今回は [Heroku](https://jp.heroku.com/) にてデプロイする方法を調べてみました。
以前、[こちら](https://zenn.dev/ryuu/articles/try-vuetifyapp)の記事で VueCLI を使って開発した SPA を実際にデプロイしていきます。

以前 Heroku にアプリをデプロイした経験はありましたが、VueSPA をデプロイした経験がなかったため、今回は Heroku にデプロイし動かせるとことまで調べてやったのでまとめてみます。

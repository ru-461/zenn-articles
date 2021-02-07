---
title: "WSL2にYarnを導入する"
emoji: "📥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["wsl2", "yarn", "nodejs"]
published: false
---

# Yarnとは？

`Yarn` とは Node.js 用のパッケージマネージャーです。

Node.js をインストールすると標準で `npm(Node Package Manager)` というパッケージマネージャーが付属します。
Node.js 向けのパッケージマネージャとしては `npm` が有名ですが、他にもパッケージマネージャーはいくつか存在します。
その中の１つに FaceBook 社が開発した Yarn というパッケージマネージャーがあります。

公式ページ
https://yarnpkg.com/

今回は Node.js をインストールした WSL2(Windows Sub System for Linux 2)の環境に Yarn を導入して動かすところまで行います。
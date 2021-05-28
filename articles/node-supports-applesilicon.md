---
title: "Node.jsがv16でAppleSiliconに対応していた"
emoji: "🍎"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["nodejs","applesilicon"]
published: false
---

# はじめに

Node.js v16 系となるバージョンが 2021 年の 4 月 20 日にリリースされました。v14 に次ぐ [LTS( Long Term Support )](https://nodejs.org/ja/about/releases/)となるバージョンになります。Node.js はメジャーバージョンが偶数になるタイミングで LTS となり、約 30 ヶ月間のサポートがされる形となっています。

またリリースしてすぐ LTS のアクティブリリースになるわけではなく、約半年後にアクティブ LTS リリースとなります。

そして今回最リリースされた新の LTS、Node.js 16 が AppleSilicon をサポートする最初のバージョンとなります。つまり M1 チップを始めとする SOC を搭載した MacBook や iMac 上にてネイティブ動作するようになったということです。これはすごいですね。

リリースノート(GitHub)はこちらになります。

https://github.com/nodejs/node/blob/master/doc/changelogs/CHANGELOG_V16.md

# さっそくインストールしてみる

それでは、Node.js 16 を実際に AppleSilicon な Mac へインストールしていきます。今回はローカルと Docker でそれぞれインストールして検証しました。

この記事の内容は執筆時点で M1 チップを搭載した Macbook Pro にて検証したものになります。

## Dockerを使って
# おわりに
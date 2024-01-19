---
title: "Expo SDKを 49 → 50 にアップグレードしてみた"
emoji: "🚀"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["expo", "reactnative"]
published: false
---

## Expo SDK 50 がやってきた

2024年1月中旬にExpo SDKのメジャーバージョンとなる50が正式に公開されました。
今回初めてExpo SDKのメジャーアップグレードを行ったので記念の意味も込めて備忘録としてまとめてみます。

[Expo](https://expo.dev)は[React Native](https://reactnative.dev)の開発支援ツールキットになります。
Expo SDKはExpoを使用するうえで便利なパッケージなどをひとまとめにしたもので、Expoを使用するうえで必須になるパッケージとなります。

Expo SDK はReact Nativeのアップグレードに伴い、React Native側のメジャーバージョンに追従する形でメジャーアップグレードがリリースされております。

今回のExpo SDK 50の目玉は昨年末にリリースされた[React Native 0.73](https://reactnative.dev/blog/2023/12/06/0.73-debugging-improvements-stable-symlinks)となります。
Expo SDK 50のベータ版然り、全体的に思っていたよりかなり前倒しでリリースされた印象があります。

この度、正式にExpo SDK 50が利用可能になったため、早速個人で開発しているプロダクトに採用してみました。

## リリースノート・Changelog

https://expo.dev/changelog/2024/01-18-sdk-50

## アップグレード

それぞれExpo SDK 50向けにパッケージをアップグレード、依存関係を解決していく形で進めます。

下記の手順について、パッケージマネージャに[Bun](https://bun.sh)を使用しています。

BunについてはExpoの公式ドキュメントにインストール手順があったため、こちらを参考に導入してセットアップしております。
https://docs.expo.dev/guides/using-bun/

`bun`ではなく`npm`を使用する場合、各コマンド対応は以下になります。

- `bun` → `npm`
- `bunx` → `npx`

### eas-cli

### expo

### Expo Go

## おわりにとこれから

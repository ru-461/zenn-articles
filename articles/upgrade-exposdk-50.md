---
title: "Expo SDKを 49 → 50 にアップグレードしてみた"
emoji: "🚀"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["reactnative", "expo"]
published: true
---

## Expo SDK 50 がやってきた

2024年1月中旬にExpo SDKのメジャーバージョンとなる50が正式に公開されました。

本家React NativeからExpoでのReact Native開発に移行し、今回初めてExpo SDKのメジャーアップグレードを行ったので記念の意味も込めて備忘録としてまとめてみます。

[Expo](https://expo.dev)は[React Native](https://reactnative.dev)のフレームワークになります。
Expo SDKはExpoを使用するうえで便利なパッケージなどをひとまとめにしたもので、Expoを使用するうえで必須になるパッケージとなります。
Expo SDKはReact Native側のアップデートに追従する形で都度アップグレードが実施されております。

今回のExpo SDK 50の目玉は昨年末にリリースされた[React Native 0.73](https://reactnative.dev/blog/2023/12/06/0.73-debugging-improvements-stable-symlinks)となります。
Expo SDK 50のベータ版然り、全体的に思っていたよりかなり前倒しでリリースされた印象があります。この度、正式にExpo SDK 50が利用可能になったため、早速個人で開発しているプロダクトに採用してみました。

## リリースノート

https://expo.dev/changelog/2024/01-18-sdk-50

## 各種アップグレード

それぞれExpo SDK 50向けにパッケージをアップグレード、依存関係を解決していく形で進めます。

下記の手順について、パッケージマネージャに[Bun](https://bun.sh)を使用しています。

BunについてはExpoの公式ドキュメントにインストール手順があったため、こちらを参考に導入してセットアップしております。
https://docs.expo.dev/guides/using-bun/

`bun`ではなく`npm`を使用する場合、各コマンド対応は以下になります。

- `bun` → `npm`
- `bunx` → `npx`

### eas-cli

Expoが提供するクラウドにてアプリのビルドやストア提出を行う際に必要となるツールになります。
開発にて導入している場合のみアップグレードが必要になります。

```shell
$ bun install --global eas-cli
```

インストール後、バージョンを確認して正常にアップグレードできていれば成功です。

```shell
$ eas --version

eas-cli/6.1.0 darwin-arm64 node-v20.9.0
```

### expo

今回のアップグレードのメイン、Expo SDKの**本体**となります。必ずアップグレードしましょう。

```shell
$ bun add expo@^50.0.0
```

### Expo Go

手持ちのスマートフォンやタブレットなどでアプリを動作させるために必要なクライアントアプリとなります。

忘れがちですが、こちらもExpo SDK 50の対応バージョンが必要なためアプリの最新化を行います。手持ちの端末ごとに各種ストアからインストール、アップグレードを行います。

- [Expo Go - App Store](https://apps.apple.com/jp/app/expo-go/id982107779)
- [Expo - Google Play](https://play.google.com/store/apps/details?id=host.exp.exponent&hl=ja&gl=US&pli=1)

アップグレード後、Expo Goのタブメニューから「Settings」を選択し、「App Info」内に記載された「Support SDKs」に`50`が含まれていることを確認します。

![Support SDKsの確認画像](/images/upgrade-exposdk-50/image01.jpg)

また、iOSシミュレータ、Androidエミュレータ内のExpo Goを使用して開発している場合は、次回起動時にExpo Goのアップデートを行うか確認されます。
`Y`と答えてインストールすることで自動アップデートが行われ、エミュレータでも問題なくExpo SDK 50での開発が可能になります。

```shell
$ bun run start

# <device-name>は利用している仮想デバイス名
Expo Go on <device-name> is outdated, would you like to upgrade?
```

Expo SDKが最新でも、Expo Goのアップデートを忘れているとアプリを動作させることができないため、まず最優先で確認したいところです。

## 依存関係の修復とチェック

### fix

ここまできたらExpo SDK 50を使用してのアプリ起動は可能になります。
しかし、Expo SDKに付随するパッケージの依存関係が最新化されていない可能性があるためチェックを行います。

```shell
$ bunx expo install --fix
```

依存関係のチェックが行われ、SDKに合わせた対応バージョンへの更新作業が行われます。
依存関係が全て解決されたあとはpackage.jsonが最新に更新されます。

警告が表示された場合は指示に従って各種依存関係の手動修正が必要になる場合があります。

### expo-doctor

アップグレード作業もいよいよ終盤です。ここまできたら最後に診断を受けて終わりになります。
Expoが提供している[expo-doctor](https://www.npmjs.com/package/expo-doctor)を使用します。


:::message
最新のSDKに対応させるため、必ず`@latest`まで指定してください。
:::

```shell
$ bunx expo-doctor@latest
```

最新リリースを使用するうえで設定やディレクトリ構成に問題がないか自動でチェックしてくれます。全てに`✓`が付いていることを確認したら完了となります。

```shell
✔ Check Expo config for common issues
✔ Check package.json for common issues
✔ Check dependencies for packages that should not be installed directly
✔ Check for common project setup issues
✔ Check npm/ yarn versions
✔ Check Expo config (app.json/ app.config.js) schema
✔ Check that native modules do not use incompatible support packages
✔ Check for legacy global CLI installed locally
✔ Check that packages match versions required by installed Expo SDK
✔ Check that native modules use compatible support package versions for installed Expo SDK

Didn't find any issues with the project!
```

`Didn't find any issues with the project!`が表示されたことで、無事にExpo SDK 50を使用する準備が完了しました。

## おわりにとこれから

今回、Expo SDK 50のメジャーアップグレードを行い、備忘録としてまとめました。
私と同じようにこれからExpoを使用したReact Nativeの開発に参入する人の参考になれれば幸いです。

メジャーアップグレードといいながら、手順がかなりわかりやすく手軽にアップグレードできました。

Expo SDKとしてExpoから提供されているパッケージを使用している範囲では大きな問題なく簡単にアップグレードできるのは大きなメリットだと感じました。
事実、React Nativeで本体のアップグレードは少し辛かった記憶があるのでExpoの恩恵をかなり受けられていると感じる瞬間でした。

Expoが大きく進化してきており、かなり開発体験が良くなってきているので今後の発展に期待ですね。

最後まで読んでいただきありがとうございました。

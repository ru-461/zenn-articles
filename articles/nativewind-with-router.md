---
title: "Expo RouterでNativeWind（Tailwind CSS）を使用する"
emoji: "🔥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["expo", "exporouter", "nativewind", "tailwindcss", "reactnative"]
published: false
---

# はじめに

React NativeのプロジェクトでTailwind CSSを使用可能にするNativeWindというライブラリがあります。

https://www.nativewind.dev/

原理としては、Web環境では`CSS StyleSheet`、Native環境では`StyleSheet.create`をそれぞれ出力しTailwind CSSを実現しているようです。

今回はExpo Routerが導入されたプロジェクトにNativeWindを新規に導入し、使用可能にするまでの流れを解説します。

# Expo-Routerの新規プロジェクト作成

Expoが提供している`create-expo-app`を利用して新規プロジェクトを作成します。
`--template`オプションをつけて実行するとテンプレートから始めることができます。

```shell
$ bunx create-expo-app nativewind-app --template
```

テンプレートの選択を求められるので、`Navigation (TypeScript)`を選択します。

```shell
? Choose a template: › - Use arrow-keys. Return to submit.
    Blank - a minimal app as clean as an empty canvas
    Blank (TypeScript)
❯   Navigation (TypeScript)
    Blank (Bare)
```

作成したプロジェクトが起動できることを確認します。

```shell
$ bun run start
```

Expo Goを使用してサンプルアプリを開けることを確認します。

![Navigationサンプル画像](/images/nativewind-with-router/image01.png)

# おわりに

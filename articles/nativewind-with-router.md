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
`--template`オプションをつけて実行すると既にExpoが用意しているテンプレートからプロジェクトを始めることができます。

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

# NativeWindセットアップ

## パッケージ導入

必須パッケージであるTailwind CSSを導入します。

```shell
$ bun add -d tailwindcss
```

導入するパッケージはこれだけです。続けてReact NativeへTailWind CSSを反映するために必要な設定を進めていきます。

## 設定ファイル作成

Tailwind CSSの設定ファイルであるtailwind.config.jsを作成します。
パッケージのインストールに続けて以下のコマンドを実行します。

```shell
$ bunx tailwindcss init --ts
```

実行するとプロジェクトのルートにtailwind.config.jsが新しく生成されます。
`--ts`としているのはTypeScriptへ対応するためになります。

生成された設定ファイルを以下のように書き換えます。

```diff javascript:tailwind.config.js
/** @type {import('tailwindcss').Config} */
module.exports = {
+  content: ["./app/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

`content: []`の箇所にTailwind CSSを適用する対象のファイルの場所を記述します。
デフォルトでは`app`となっていますが、トップディレクトリはプロジェクトに応じて適宜修正してください。

Expo RouterではNext.jsなどのフレームワークでよく見られる、srcディレクトリが標準でサポートされています。

https://docs.expo.dev/router/reference/src-directory/

```diff javascript:tailwind.config.js
/** @type {import('tailwindcss').Config} */
module.exports = {
-  content: ["./app/**/*.{js,jsx,ts,tsx}"],
+  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

## CSSファイル作成

Tailwind CSSで使用するCSSファイルを新規作成します。

```shell
$ touch ./tailwind.css
```

以下の3行を記述します。

```css:tailwind.css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

これにてNativeWindの導入は完了です。プロジェクト内でTailwind CSSを使用する準備ができました。

# おわりに

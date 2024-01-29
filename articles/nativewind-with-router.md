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
$ bunx expo install nativewind@^4.0.1 react-native-reanimated tailwindcss
```

導入するパッケージはこれだけです。続けてReact NativeへTailWind CSSを反映するために必要な設定を進めていきます。

## 設定ファイル作成

Tailwind CSSの設定ファイルであるtailwind.config.jsを作成します。
パッケージのインストールに続けて以下のコマンドを実行します。

```shell
$ bunx tailwindcss init
```

実行するとプロジェクトのルートにtailwind.config.jsが新しく生成されます。
`--ts`としているのはTypeScriptへ対応するためになります。

生成された設定ファイルを以下のように書き換えます。

```diff javascript:tailwind.config.js
/** @type {import('tailwindcss').Config} */
module.exports = {
+  content: ["./app/**/*.{js,jsx,ts,tsx}"],
+  presets: [require("nativewind/preset")],
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
  presets: [require("nativewind/preset")],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

## CSSファイル作成

Tailwind CSSで使用するCSSファイルを新規作成します。

```shell
$ touch ./global.css
```

以下の3行を記述します。

```css:global.css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

## Babelの設定変更

babel.config.jsを修正します。以下の行を追加します。

```diff javascript:babel.config.js
module.exports = function (api) {
  api.cache(true);
  return {
-   presets: ['babel-preset-expo']
+   ["babel-preset-expo", { jsxImportSource: "nativewind" }],
+   "nativewind/babel",
  };
};
```

## Metroの設定変更

metro.config.jsの設定を変更します。
プロジェクトルートにmetro.config.jsが存在しない場合は下記のコマンドで生成できます。

```shell
$ bunx expo customize metro.config.js
```

生成されたmetro.config.jsを下記のように書き換えます。

```javascript:metro.config.js
const { getDefaultConfig } = require("expo/metro-config");
const { withNativeWind } = require('nativewind/metro');

const config = getDefaultConfig(__dirname)

module.exports = withNativeWind(config, { input: './global.css' })
```

## 型参照の追加

TypeScriptで正しく形定義の参照ができるようプロジェクトのルートにnativewind-env.d.tsを新規作成します。

```shell
$ touch nativewind-env.d.ts
```

下記の定義を追加します。

```typescript:nativewind-env.d.ts
/// <reference types="nativewind/types" />
```

これにてNativeWindの導入は完了です。プロジェクト内でTailwind CSSを使用する準備ができました。

# おわりに

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

原理としては、Web環境では`CSS StyleSheet`、Native環境では`StyleSheet.create`をそれぞれ出力しTailwind CSSを実現しているようです。Native環境で使用する場合、Webで使用可能なユーティリティクラスが一部使えないなどの制約がありますが現在開発が進められており日々進化を続けています。

今回はExpo Routerが導入されたプロジェクトにNativeWindを新規に導入し、使用可能になるまでの流れを解説します。

:::message
2024年初頭にExpo Router V3が登場し、記事公開時点ではNativeWindはV4への移行途中となっております。破壊的変更が入る可能性もあるため、不明点については[公式ドキュメント](https://www.nativewind.dev/v4/overview)も併せてご確認ください。
:::

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

以下のようにbabel.config.jsを修正します。

```diff javascript:babel.config.js
module.exports = function (api) {
  api.cache(true);
  return {
-   presets: ['babel-preset-expo']
+   presets: [
+     ["babel-preset-expo", { jsxImportSource: "nativewind" }],
+     "nativewind/babel",
+   ],
+   plugins: ['react-native-reanimated/plugin'],
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

## CSSのインポート

アプリのエントリーポイントとなるコンポーネントで先ほど作成したglobal.cssをインポートします。
Expo Routerの場合はデフォルトでapp/_layout.tsxが一番親のコンポーネントになります。

_layout.tsxの先頭へ下記の一行を追加します。

```tsx:_layout.tsx
import "@/global.css"
```

これにてExpo RouterへNativeWindの導入は完了です。プロジェクト内でTailwind CSSを使用する準備ができました。

## NativeWindを試す

導入したNativeWindを使用して簡単なページを作成します。
Expo Routerではappディレクトリを起点としてルーティングが自動生成されます。

(tab)グループの中に新しいルート（/wind）を作成します。

```shell
$ touch app/(tabs)/wind.tsx
```

Tab Windページの内容を記載します。

```tsx:wind.tsx
import { MaterialCommunityIcons } from "@expo/vector-icons";
import { Text, View } from "@/components/Themed";

export default function WindScreen() {
  return (
    <View className="flex-1 items-center justify-center">
      <MaterialCommunityIcons size={48} name="tailwind" color="#1AB3BA" />
      <Text className="text-2xl font-bold underline">Hello, NativeWind!</Text>
    </View>
  );
}
```

タブバーへボタンを新規追加します(コードが長くなるため一部抜粋)
`<Tabs.Screen />`を複製し末尾に新しいタブを追加してください。

```tsx:_layout.tsx
      // 抜粋
      <Tabs.Screen
        name="two"
        options={{
          title: 'Tab Two',
          tabBarIcon: ({ color }) => <TabBarIcon name="code" color={color} />,
        }}
      />
      <Tabs.Screen
        name="wind"
        options={{
          title: 'Tab Wind',
          tabBarIcon: ({ color }) => <TabBarIcon name="code" color={color} />,
        }}
      />
```

アプリのタブバーに新しく`Tab Wind`タブが追加されます。

![新しくタブが追加された様子の画像](/images/nativewind-with-router/image02.png)

新しく追加された`Tab Wind`をタップしてページを開きます。
以下のようにTailwind CSSでスタイルが当たることを確認します。

![新しくページが追加された様子の画像](/images/nativewind-with-router/image03.png)

# おわりに

今回は、Expo RouterのプロジェクトにNativeWindを導入し、Tailwind CSSを利用できるようにするまでの手順を解説しました。導入手順がWebと比べて少し複雑ではありますが、ユーティリティファーストでアプリ開発を進めることが可能になるので検討する価値があると感じました。

まだまだWeb向けのTailwind CSSよりも情報が少ないですが、React NativeでもTailwind CSSを利用できるのは大きな魅力だと感じました。うまく活用して開発効率を上げていきましょう。

最後まで読んでいただきありがとうございました。

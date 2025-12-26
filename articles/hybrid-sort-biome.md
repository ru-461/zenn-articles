---
title: "【Next.js】BiomeとESLintのハイブリッド構成について考える（暫定版）"
emoji: "🏞️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["biome", "eslint", "nextjs"]
published: true
---

## はじめに

:::message
本記事の内容は[Biome 1.5.3](https://biomejs.dev/ja/internals/changelog/#153-2024-01-22)時点での暫定対応になります。

Biomeでまだ実装されていないルールを[ESLint](https://eslint.org)で補おうという趣旨のため予めご了承ください。今後のBiomeのアップデートによって全てBiomeで対応可能になる可能性があります。
:::

普段、Biomeをメインのリンター・フォーマッターとしてNext.js 14のプロジェクトにて使用しています。ESLintと互換性のあるルールが多く実装されておりどんどん移行しやすくなってきている印象を受けます。
https://biomejs.dev

Biomeが注目されたことで脱ESLintや脱Prettierに向けて動いているプロジェクトも多そうですね。
私も実際に環境構築を試みましたが、まだ一部のルールがBiomeで未実装のため、必要に応じてESLintを併用することがあります。

日々ESLintからBiomeへの移行を進めながら、一時的ながらもESLintとBiomeを組み合わせたハイブリッドアプローチを取ることができたため記事にします。
本構成としてはBiomeとESLint 9:1 ぐらいの比率で構成し、メインとしてBiomeを使いつつ、不足しているルールをESLintでカバーすることを目的としています。

## ESLint併用に至った経緯

ESLintを使用してコードのソートまで行える有名なルールに下記の2つがあります。

- [import-js/eslint-plugin-import](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/order.md)
- [jsx-eslint/eslint-plugin-react](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-sort-props.md)

それぞれ、import、propsを解析、並び替えを提案、`--fix`を使用しての自動修正まで実行可能なルールになります。
これらは、Next.jsの新規プロジェクトセットアップ記事などでよくおすすめされていることもあり導入されているプロジェクトをよく見ます。

しかしBiome 1.5.3現在、**上記ルールと完全に互換するルールは存在しません**。

補足しておくと、[eslint-plugin-import](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/order.md)相当の機能はBiomeでは"Analyzer imports-sorting"として実装されています。
https://biomejs.dev/analyzer/#imports-sorting

Biomeの機能としてimport文に空行を入れてグルーピングすることでグループ単位でのソート提案、自動修正をしてくれます。
しかし、上記の機能では[eslint-plugin-import](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/order.md)が提供するimportのグループ化までは自動で行ってくれないため開発者自身でグループ分けする必要があります。

都度、自分でグループ分けしてもいいのですが、importがファイル内に増えてくると手動ではどうしても管理しづらいという問題があります。

そこで、ESLintの力を享受する名付けてソートハイブリッドアプローチを提案します。

## Biomeのセットアップ

[create-next-app](https://nextjs.org/docs/pages/api-reference/create-next-app)を使用してNext.jsのプロジェクトがセットアップされているものとします。

プロジェクトを生成する際`Would you like to use ESLint?`に`Yes`で答えた場合、ESLintが自動インストールされ、以下の.eslintrc.jsonが自動生成されます。

```json:.eslintrc.json
{
  "extends": "next/core-web-vitals"
}
```

本構成ではBiomeをメインで使用するため、上記のJSONファイルをベースにして**必要なルールのみ**を追加します。

### Biomeのインストール

本記事内ではパッケージマネージャに[Bun](https://bun.sh)を使用します。ここは適宜お使いのパッケージマネージャに読み替えて実行してください。

まず、Biomeをインストールします。

```shell
$ bun add --dev --exact @biomejs/biome
```

### 設定ファイルの作成

Biomeの設定に必要なファイル（biome.json）を生成します。

```shell
$ bunx @biomejs/biome init
```

プロジェクトルートに下記のJSONファイルが生成されます。Biomeの設定はリンターとフォーマットまでが1つのJSONファイルによって定義されます。

```json:biome.json
{
  "$schema": "https://biomejs.dev/schemas/1.5.3/schema.json",
  "organizeImports": {
    "enabled": false
  },
  "linter": {
    "enabled": true,
    "rules": {
      "recommended": true
    }
  }
}
```

## Biomeの設定

続けてBiomeの設定をします。設定可能な項目については以下を参考にしてください。
https://biomejs.dev/ja/linter/rules/

ESLintやPrettierで提供されているルールは一通り網羅されています。ルールによってはオプションがあるためドキュメントを参照してください。

リンターのルールはデフォルトで`"recommended": true`となっており、推奨されるルールが一通り適用されています。またルールを追加したり上書きしたりできます。
ここはプロジェクトに応じて適宜変更してみてください。

一応、参考として私がNext.jsのプロジェクトにてよく使用している設定を掲載します。

```json:biome.json
{
  "$schema": "https://biomejs.dev/schemas/1.5.3/schema.json",
  "organizeImports": {
    "enabled": true
  },
  "formatter": {
    "enabled": true,
    "ignore": [".next", "node_modules"],
    "indentStyle": "space"
  },
  "javascript": {
    "formatter": {
      "quoteStyle": "single"
    }
  },
  "linter": {
    "enabled": true,
    "ignore": [".next", "node_modules"],
    "rules": {
      "recommended": true,
      "correctness": {
				"noUnusedTemplateLiteral": "off",
        "noUnusedVariables": "error",
        "useHookAtTopLevel": "error"
      },
      "nursery": {
        "noUnusedImports": "warn"
      },
      "style": {
        "noNegationElse": "error",
        "useBlockStatements": "error"
      }
    }
  }
}
```

今回Biomeのimport最適化機能を使用したいため必ず、`organizeImports`は`"enabled": true`にして有効化してください。

私自身、Next.jsでBiomeを使用し始めたばかりなので、もっとおすすめの構成があれば教えて下さい。

## ESLintの構成

上記のBiomeでフォーマットとリンターの設定は完了したのですが、import文のソートと、propsのソートがまだできないため、不足しているルールをESLintで補います。
デフォルトでインストールされている[eslint-config-next](https://nextjs.org/docs/app/building-your-application/configuring/eslint#eslint-config)を拡張し、Biomeにて提供されていないルールを補う形になります。

### JSXのpropsをソート

JSX（TSX）内にてpropsを解析し最適な並び順を提案します。また、`--fix`を使用して自動ソートも可能となります。

ルールを有効化するため、.eslintrc.json内に以下を追記します。

```diff json:.eslintrc.json
{
  "extends": "next/core-web-vitals",
+ "rules": {
+   "react/jsx-sort-props": "warn"
+ }
}
```

上記の設定後、下記のコマンドでpropsのソートが可能になります。

```shell
$ bunx eslint './src/**/*.{js,jsx,ts,tsx}' --fix
```

例として以下のコンポーネントがある場合。

```tsx:src/app/loading.tsx
export default function Loading() {
  return (
    <Box display="flex" h="100vh" justifyContent="center" alignItems="center">
      <Spinner size="xl" color="#10B981" />
    </Box>
  );
}
```

コマンド実行後に**アルファベット順**でpropsがソートされます。

```tsx:src/app/loading.tsx
export default function Loading() {
  return (
    <Box alignItems="center" display="flex" h="100vh" justifyContent="center">
      <Spinner color="#10B981" size="xl" />
    </Box>
  );
}
```

### importの自動グループ化

importの種別毎にグループ化、並び替えを行います。
ルールを有効化するため、.eslintrc.json内に以下を追記します。

```diff json:.eslintrc.json
{
  "extends": "next/core-web-vitals",
  "rules": {
+   "import/order": [
+     "warn",
+     {
+       "alphabetize": {
+         "caseInsensitive": true,
+         "order": "asc"
+       },
+       "groups": [
+         "builtin",
+         "external",
+         "internal",
+         "parent",
+         "sibling",
+         "index",
+         "object",
+         "type"
+       ],
+       "newlines-between": "always",
+       "pathGroupsExcludedImportTypes": ["builtin"]
+     }
+   ],
  "react/jsx-sort-props": "warn"
  }
}
```

上記の設定で一番重要なのは`"newlines-between": "always",`の部分になります。
後述するVSCodeのESLint拡張機能を使用する場合、`"groups"`の定義がないと自動でグループ化されません。必ず定義するようにします。

グループ化したうえで、グループ毎に空行を入れてくれるため、BiomeのorganizeImportsとうまく組み合わせられるようになります。
ソート順については、Biomeのもつ[Analyzer](https://biomejs.dev/analyzer/#imports-sorting)と合わせるため、`"alphabetize":`をアルファベット順（`asc`）としています。

また、importのグループ化について細かく設定ができるため、好みに合わせて調整してみてください。
私は以下のような設定をしています。

```json:.eslintrc.json
{
  "extends": "next/core-web-vitals",
  "rules": {
    "import/order": [
      "warn",
      {
        "alphabetize": {
          "caseInsensitive": true,
          "order": "asc"
        },
        "groups": [
          "builtin",
          "external",
          "internal",
          "parent",
          "sibling",
          "index",
          "object",
          "type"
        ],
        "newlines-between": "always",
        "pathGroups": [
          {
            "group": "builtin",
            "pattern": "next/**",
            "position": "before"
          },
          {
            "group": "parent",
            "pattern": "@/components/**",
            "position": "before"
          },
          {
            "group": "type",
            "pattern": "@/types",
            "position": "before"
          }
        ],
        "pathGroupsExcludedImportTypes": ["builtin"]
      }
    ],
    "react/jsx-sort-props": "warn"
  }
}
```

propsのソートと同様に以下のコマンドでESLintを使用した自動ソートが可能になります。

```shell
eslint './src/**/*.{js,jsx,ts,tsx}' --fix
```

例えば以下のようなコンポーネントのimportがある場合。

```tsx:src/app/layout.tsx
import type { Metadata } from 'next';
import { ChakraProvider } from '@chakra-ui/react';
import { Header } from '@/components/Header';
import { Main } from '@/components/Main';
import { Footer } from '@/components/Footer';
...
```

ESLintによって以下のようにソート、グルーピングされます。

```tsx:src/app/layout.tsx
import { ChakraProvider } from '@chakra-ui/react';

import { Footer } from '@/components/Footer';
import { Header } from '@/components/Header';
import { Main } from '@/components/Main';

import type { Metadata } from 'next';
...
```

## VSCodeとの統合

VSCodeで開発する際に、自動でBiomeのフォーマットとリンターを適用したいので拡張機能を導入します。

以下の拡張機能をVSCodeのMarketplaceから導入します。
https://marketplace.visualstudio.com/items?itemName=biomejs.biome

また、ESLintの拡張機能も合わせて導入します。
https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint

導入後、以下のコマンドでVSCodeの設定ファイル`.vscode/settings.json`を作成します。既に作成してある場合は不要です。

```shell
$ mkdir -p ./.vscode && touch ./.vscode/settings.json
```

settings.jsonへ以下を記載します。

```json:.vscode/settings.json
{
  "editor.codeActionsOnSave": {
    "quickfix.biome": "explicit",
    "source.addMissingImports": "explicit",
    "source.fixAll.eslint": "explicit",
    "source.organizeImports.biome": "explicit"
  },
  "editor.defaultFormatter": "biomejs.biome",
  "editor.formatOnPaste": true,
  "editor.formatOnSave": true
}
```

これでVSCodeにてファイルの保存をしたタイミングで自動的にBiomeとESLintによるフォーマットが行われるようになります。
また任意ですが、コードをペーストしたタイミングでもフォーマットされるようにしています。お好みで設定は調整してください。

## スクリプト追加

コマンドから実行するために、フォーマット用のスクリプトをpackage.jsonに記載しておきます。
私の設定例を紹介しますが、こちらも適宜使いやすいように調整してください。

```diff json:package.json
"scripts": {
  ...
+ "check": "biome check --apply ./src && eslint './src/**/*.{js,jsx,ts,tsx}' --fix",
},
```

Biome CLIが提供している[biome check](https://biomejs.dev/ja/reference/cli/#biome-check)とESLintを同時に実行します。
https://biomejs.dev/ja/reference/cli/#biome-check

`biome check`はフォーマットとリントを同時に行ってくれる便利なコマンドになっております。
また、`--apply`をつけることで修正可能なものを見つけて自動修正してくれます。

上記のスクリプトでは`biome check`と合わせて`eslint`を実行します。
eslintは`--fix`としているため、ESLintによってimportのグループ化とソート、propsの自動ソートが実行されます。

これにより、コミットする前に`bun run check`を実行することでコードの粒度を保つことができます。

また、[husky](https://typicode.github.io/husky/)や[lint-staged](https://github.com/lint-staged/lint-staged)などを導入してコミット前に自動実行させるのもおすすめです。

## おわりに

今回は、BiomeとESLintを併用するコードの自動ソートのハイブリッドアプローチを紹介しました。

自分のユースケースでは完全にBiomeに移行できなかったため暫定対応とはなりますが、現在大きな問題なく運用できています。

Next.jsでは長らくPrettierによるコードのフォーマットとESLintによる静的解析がデファクトスタンダードになっていましたが、今後Biomeに乗り換えるプロジェクトも増えそうです。
Biomeも日々アップデートが行われESLintのルールカバー率がどんどん向上しているため今後全てBiomeの設定だけで完結できるようになる日も近そうです。

他にもTailwind CSSの`className`のソートサポートなど実現して欲しいルールが多くあるため今後も情報を追っていきたいところです。

最後まで読んでいただきありがとうございました。

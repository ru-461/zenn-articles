---
title: "【Next.js】BiomeとESLintを組み合わせてコードをソートしたい"
emoji: "🤖"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["biome", "eslint", "nextjs"]
published: false
---

## はじめに

:::message
本記事の内容はBiome 1.5.3 時点での暫定対応になります。
Biomeでまだ実装されていないルールをESLintで補おうという趣旨のため予めご了承ください。
今後のBiomeのアップデートによって全てBiomeで対応可能になる可能性があります。
:::

普段、Biomeをメインのリンター・フォーマットとしてNext.js 14のプロジェクトにて早速導入して使用しています。ESLintのルールと互換性がバージョンアップごとに向上しており、すぐ移行でき結構気に入っております。

https://biomejs.dev

Biomeが注目されたことで脱ESLintや脱Prettierに向けているプロジェクトも多そうですね。私も実際に環境構築をしてみたのですが、まだ一部のルールがBiomeにて対応していないため適宜ESLintを併用したいと感じることがありました。

日々ESLintからBiomeへの移行を進める中で、暫定ではありますが、ESLintとBiomeを組み合わせて自分なりのハイブリッドアプローチができたので共有いたします。

## Biomeでカバー

ESLintにてよく設定される人気のルールに下記の2つがあります。

- [import-js/eslint-plugin-import](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/order.md)
- [jsx-eslint/eslint-plugin-react](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-sort-props.md)

それぞれ、import、propsを解析し、並び替えを提案、自動ソートしてくれるものになります。これらは、Next.jsの新規プロジェクトセットアップ記事などでよくおすすめされていることもあり使用しているプロジェクトをよく見ます。

しかしBiome 1.5.3現在、上記のルールと完全に互換するルールは存在しません。

補足しておくと、eslint-plugin-import相当の機能はBiomeに存在します。import文に空行を入れてグルーピングすることでグループ単位でのソート提案、自動修正をしてくれます。

https://biomejs.dev/analyzer/

しかし、上記の機能ではimportのグルーピングは行ってくれないため開発者が目視で並べ替えする必要があります。


## Biomeのセットアップ

予め、[create-next-app](https://nextjs.org/docs/pages/api-reference/create-next-app)を使用してNext.jsのプロジェクトがセットアップされているものとします。
このとき、ESLintの設定まで完了し、以下の.eslintrc.jsonがプロジェクトルートに存在していることを前提とします。

デフォルトでは以下の.eslintrc.jsonが作成されています。

```json:.eslintrc.json
{
  "extends": "next/core-web-vitals"
}
```

### Biomeのインストール

本記事内ではパッケージマネージャにBunを使用します。適宜お使いのパッケージマネージャに読み替えて実行してください。

Biomeをインストールします。

```shell
$ bun add --dev --exact @biomejs/biome
```

### 設定ファイルの作成

Biomeの設定に必要なファイル（biome.json）を生成します。

```shell
$ bunx @biomejs/biome init
```

プロジェクトルートに下記のJSONファイルが生成されます。Biomeの設定はリンターとフォーマットとまで全てが1つのJSONファイルによって定義されます。

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

続けてBiomeの設定をします。
設定可能な項目については以下を参考にしてください。

https://biomejs.dev/ja/linter/rules/

ESLintやPrettierで提供されているルールは一通り網羅されています。ルールによってはオプションがあるためドキュメントを参照してください。

リンターのルールはデフォルトで`"recommended": true`となっており、メジャーなリンタールールは一通り適用されています。足りない設定があれば続けて記載することで追加したり上書きしたりできます。

一応、私がNext.jsのプロジェクトにてよく使用している設定を掲載します。

```json:biome.json
{
  "$schema": "https://biomejs.dev/schemas/1.5.3/schema.json",
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
  },
  "organizeImports": {
    "enabled": true
  }
}
```

今回Biomeのimport最適化機能を使用したいため必ず、`organizeImports`は`"enabled": true`にして有効化してください。

Next.jsでBiomeを使用し始めたばかりなので、もっとおすすめの構成があれば教えて下さい。

## ESLintの構成

上記のBiomeでフォーマットとリンターの設定は完了したのですが、import文のソートと、propsのソートがまだできないため、不足しているルールをESLintで補います。

デフォルトでインストールされているNext.jsのESLintルール（next/core-web-vitals）を拡張しBiomeにて提供されていないルールを補う形になります。

### JSXのpropsをソート

JSX（TSX）内にてpropsを解析し並び順を警告します。
`--fix`を使用して自動フォーマットもできるようになります。

.eslintrc.jsonに以下を追記します。

```diff:json:.eslintrc.json
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

### importの自動グループ化

.eslintrc.jsonに以下を追記します。

```diff:json:.eslintrc.json
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
+       "newlines-between": "always",
+       "pathGroupsExcludedImportTypes": ["builtin"]
+     }
+   ],
  "react/jsx-sort-props": "warn"
  }
}
```

上記の設定で一番重要なのは`"newlines-between": "always",`の部分になります。
importをグループ化し、適宜空行をいれてくれるため、BiomeのorganizeImportsとうまく組み合わせられるようになります。
ソート順については、BiomeのorganizeImportsと合わせるため、`"alphabetize":`をアルファベット順（`asc`）としています。

またimportのグループ化ついて細かく設定ができるため。好みに合わせて調整してみてください。
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

## VSCodeとの統合

VSCodeで開発する際に、自動でBiomeのフォーマットとリンターを適用したいので拡張機能を導入します。
以下の拡張機能をMarketplaceから導入します。

https://marketplace.visualstudio.com/items?itemName=biomejs.biome

導入後、以下のコマンドでVSCodeの設定ファイルを作成します。既に作成してある場合は不要です。

```shell
mkdir -p ./vscode && touch ./vscode/settings.json
```

settings.jsonへ以下を記載します。

```json:./vscode/settings.json
{
  "editor.codeActionsOnSave": {
    "quickfix.biome": "explicit",
    "source.organizeImports.biome": "explicit",
  },
  "editor.defaultFormatter": "biomejs.biome",
  "editor.formatOnPaste": true,
  "editor.formatOnSave": true
}
```

これでVSCodeにてファイルの保存をしたタイミングで自動的にBiomeによるフォーマットが行われるようになります。
また任意ですが、コードをペーストしたタイミングでもフォーマットがかかるようにしています。お好みで設定は調整してください。

## スクリプト追加

コマンドから実行するために、フォーマット用のスクリプトをpackage.jsonに記載しておきます。
私の設定例を紹介しますが、こちらも適宜使いやすいように調整してください。

```shell:package.json
"scripts": {
  ...
+ "check": "biome check --apply ./src && eslint './src/**/*.{js,jsx,ts,tsx}' --fix",
},
```

Biome CLIが提供している[biome check](https://biomejs.dev/ja/reference/cli/#biome-check)とESLintを同時に実行します。
`biome check`はフォーマットとリントを同時に行ってくれる便利なコマンドになっております。
また、`--apply`をつけることで修正可能なものを見つけて自動修正してくれます。

上記のスクリプトでは`biome check`と合わせて`eslint`を実行します。
eslintは`--fix`としているため、ESLintによってimportのグループ化とソート、propsの自動ソートが実行されます。

これにより、コミットする前に`bun run check`と実行することでコードの粒度を保つことができます。

### 本構成の注意点

VSCodeでのコード編集時はBiomeの拡張機能によって、リンターとフォーマッター処理のみが行われます。
ESLintの処理は自動的に実行されないため、`eslint './src/**/*.{js,jsx,ts,tsx}' --fix`をコミット前に忘れず実行することが必要となります。

[huskey](https://typicode.github.io/husky/)や[lint-staged](https://github.com/lint-staged/lint-staged)などを導入してコミット前に自動実行させるのもおすすめです。

## おわりに

今回BiomeとESLintを組み合わせたハイブリッドアプローチを紹介しました。

自分のユースケースでは完全にBiomeに移行できなかったため暫定対応とはなりますが、現在大きな問題なく運用できています。

Next.jsでは長らくPrettierによるコードのフォーマットとESLintによる静的解析がデファクトスタンダードになっていましたが、今後Biomeに乗り換えるプロジェクトも増えそうです。
Biomeも日々アップデートが行われESLintのルールカバー率がどんどん向上しているため今後全てBiomeの設定だけで完結できるようになる日も近そうです。

他にもTailwind CSSの`className`のソートサポートなど実現して欲しいルールが多くあるため今後も情報を追っていきたいところです。

最後まで読んでいただきありがとうございました。

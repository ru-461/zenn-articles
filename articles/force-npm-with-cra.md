---
title: "Create React Appで使うパッケージマネージャをnpmに強制する"
emoji: "💼"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["react", "createreactapp","npm"]
published: true
---

:::message
この記事の内容は[Create React App v5.0.0](https://github.com/facebook/create-react-app/releases/tag/v5.0.0)以前の内容です。現在はデフォルトでnpmをパッケージマネージャとして使用するように仕様変更されています。

Yarnを引き続き使うには`yarn create react-app <project-name>`でプロジェクトを生成できます。
:::

## はじめに

[Create React App](https://create-react-app.dev)ではローカルにYarnがインストールされているとデフォルトでパッケージマネージャとしてYarnが使われます。**Yarnがローカルにインストールされている状態**で`npx crate-react-app my-app`とした場合は以下のように表示されYarnで起動するコマンドが促されます。

```shell
# Yarnがローカルに存在
$ which yarn
/opt/homebrew/bin/yarn

# Create React Appでアプリを作成
$ npx create-react-app my-app

...

We suggest that you begin by typing:

  cd my-app2
  yarn start

Happy hacking!
```

**Yarnがローカルにインストールされていない場合はnpmを使用する**ようになり、プロジェクト生成後に`npm start`で開発サーバーを立ち上げることを促されます。

```shell
# Yarnがローカルに存在しない
$ which yarn
yarn not found

# Create React Appでアプリを作成
$ npx create-react-app my-app

...

We suggest that you begin by typing:

  cd my-app
  npm start

Happy hacking!
```

Yarnがインストールされている状態で`npx`を使用して作成したReactプロジェクトではYarnの利用を想定してYarn.lockが生成されています。`npm start`でもアプリを立ち上げることは可能ですが、npmの利用を強制するところまでは至りません。

そこでnpmを使用してのパッケージ管理を強制したい場合にできることを紹介します。

## オプションでnpmを強制させる

Create react Appにはパッケージマネージャをnpmに固定するオプションがあります。

```shell
$ npx create-react-app my-app --use-npm
```

上のように`--use-npm`というオプションを付けることでYarnがインストールされている状態でもnpmをデフォルトで使用するようにできます。

```shell
# Yarnがローカルに存在
$ which yarn
/opt/homebrew/bin/yarn

# npmを強制するオプションをつけて作成
$ npx create-react-app my-app --use-npm

...

We suggest that you begin by typing:

  cd my-app
  npm start

Happy hacking!

```

Yarnがインストールされている環境でもnpmを使用するように設定できました。`--use-npm`をつけて作成した場合、yarn.lockではなくpackage-lock.jsonがプロジェクト配下に生成されます。

```shell
$ cd my-app
# --use-npmをつけて作成した場合の構成
$ ls
node_modules  package-lock.json  package.json  public  README.md  src
```

## おわりに

YarnはReactの開発元であるMeta社（旧Facebook社）が開発していることもあり、Crate React AppがデフォルトでYarnを選択する仕様には納得です。npmを使用したい場合でも公式からオプションが提供されており、npmを使用するケースがあった場合にも柔軟に対応できるのが便利だと感じました。

最後まで読んでいただきありがとうございました。

## 参考

- [Getting Started | Create React App](https://create-react-app.dev/docs/getting-started/#selecting-a-package-manager)
- [reactjs - Switch to npm for create-react-app - Stack Overflow](https://stackoverflow.com/questions/51048173/switch-to-npm-for-create-react-app)

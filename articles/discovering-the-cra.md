---
title: "Create React Appはカレントディレクトリに直接展開できる"
emoji: "💡"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["react", "createreactapp"]
published: true
---

## はじめに

Reactで手早くプロジェクトの雛形を作る際に[Create-React-App](https://create-react-app.dev)を使用する例は多いです。Reactの開発をしているMeta社が開発しており、Reactチュートリアルでも使用が推奨されています。
https://ja.reactjs.org/docs/create-a-new-react-app.html

Node.jsを実行できる環境で以下のコマンドを実行することでReactのプロジェクトを生成できます。

```shell
$ npx create-react-app my-app
```

この例だとコマンドを実行しているディレクトリ直下に`my-app`というディレクトリが生成されます。

今回、CreateReactAppを使ったプロジェクト生成でちょっとした発見があったためここに備忘録としてまとめておきます。

## カレントディレクトリ内にプロジェクトを生成したい

プロジェクトを生成するときにプロジェクト名をコマンドの後ろにつけるのですが、ここである疑問が浮かびました。それは**プロジェクトのディレクトリ内にプロジェクトを生成するのではなく、プロジェクトをカレントディレクトの中に展開できないのか**というものです。

CreateReactAppの[公式ドキュメント](https://create-react-app.dev/docs/getting-started)を見たところ全てプロジェクト名（my-app）を指定して作成していました。プロジェクト生成後に`cd my-app`としてプロジェクトに移動してビルドする流れが紹介されており、カレントディレクトにプロジェクトの中身（my-app）の中身を展開するやり方の紹介がありませんでした。

そこで直感的にカレントディレクトリ（.）をしたところカレントディレクトリにプロジェクトを生成できました。

以下は`react-projects`という親ディレクトリにmy-appというプロジェクトを作成する例です。

```shell
# 先にプロジェクトを格納するディレクトリを作成して移動
$ mkdir my-app
$ cd my-app

# my-app内にReactプロジェクトを生成
$ npx create-react-app .

Creating a new React app in react-projects/my-app.
...
cd react-projects/my-app
  npm start

Happy hacking!
```

上のようにすることでカレントディレクトリ（my-app）にReactプロジェクトの中身だけを作成できます。

```shell
# 場所
/Users/user/react-projects/my-app

# フォルダ構成を確認
$ tree -L 1
.
├── README.md
├── node_modules
├── package-lock.json
├── package.json
├── public
└── src

3 directories, 3 files
```

結果として`/react-projects`内で`npx create-react-app my-app`したのと同じになります。カレントディレクトリにプロジェクトファイルが配置されているため、続けて`npm start`すると開発サーバーが立ち上がります。また、`create-react-app`のあとに続けて階層を指定すると作成先のファイル階層をさらに深くしたりと自由に作成先を指定できるので覚えておくと便利です。

```shell
# カレントディレクトリにreact-projects/my-react-app/appを作成
$ npx create-react-app react-projects/my-react-app/app
```

## おわりに

今回、CreateReactAppでカレントディレクトリにプロジェクトを展開するやり方についてまとめました。DockerでReactの環境を作る際にディレクトリ構成で迷いがちだったので明示的にプロジェクトを指定して展開できるとわかりやすいですね。
公式ドキュメントに書いてないので、このやり方がレアケースかなとは感じておりますが誰かの参考になれば幸いです。

最後まで読んでいただきありがとうございました。

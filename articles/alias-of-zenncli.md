---
title: "Zennでの執筆体験を加速させるエイリアス設定"
emoji: "🎢"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["zenn", "zenncli"]
published: true
---

## はじめに

Zennでは[Zenn CLI](https://zenn.dev/zenn/articles/install-zenn-cli)と呼ばれるツールが公式から提供されており、GitHubを利用した記事の投稿、記事のバージョン管理ができます。ブラウザ上のエディタに縛られない柔軟な執筆環境を構築できる点がZennの大きな特徴です。今回は、Zenn CLI用のエイリアスを作成することで記事の執筆に集中できる環境を作成しました。

## 作成したエイリアスの一覧

:::message
以下のエイリアスはZshでの利用を前提としています。また、記事を管理するディレクトリへのパスは環境によって各自読み替えてください。
:::

```shell:.zshrc
# Zenn CLI用にに定義したエイリアスの確認用
alias agzenn='alias | grep zenn'
# Zennのコンテンツ管理に移動
alias zenn='cd ~/Documents/my-zenn-contents'
# Zennのコンテンツ管理ディレクトリに移動してVSCodeで開く & プレビュー表示
alias zennop='zenn && code ~/Documents/my-zenn-contents && npx zenn preview --open'
# 新しい記事をランダムなスラッグで作成
alias zennna='zenn && npx zenn new:article'
# 新しい記事をスラッグを指定して作成
alias zennnas='zenn && npx zenn new:article --slug'
# 新しい本をランダムなスラッグで作成
alias zennnb='zenn && npx zenn new:book'
# 新しい本をスラッグを指定して作成
alias zennnbs='zenn && npx zenn new:book --slug'
# ブラウザ上でプレビューを開く
alias zennpr='zenn && npx zenn preview --open'
# Zenn CLIのバージョン確認
alias zennv='zenn && npx zenn --version'
# Zenn CLIのアップデート
alias zennup='zenn && npm install zenn-cli@latest'
```

## すぐ執筆環境を立ち上げるオールインワンエイリアス

Zennの執筆をすぐ行いたいとき用のエイリアスを定義したら以下のようになりました。
コマンドを`&&`繋いでおり、かなりごちゃごちゃしていますが、個人的なオールインワンエイリアスになります。

```shell:.zshrc
# Zennのコンテンツ管理ディレクトリに移動
alias zenn='cd ~/Documents/my-zenn-contents'
# VSCodeで開くと同時にブラウザでプレビュー
alias zennop='zenn && code ~/Documents/my-zenn-contents && npx zenn preview --open'
```

上記を実行することで以下の動作が順番に実行されます。

1. Zennの作業ディレクトリに移動
2. VSCodeで開く
3. ブラウザを起動し記事をプレビュー

Zenn CLIを使って記事をすぐに書き始められる環境がコマンド1つで立ち上がるのは地味に便利です。

## コンテンツ管理ディレクトリへのアクセス

記事を管理しているディレクトリ（my-zenn-contents）へすぐ移動できるように`zenn`というエイリアスを設定しています。

```shell:.zshrc
# ドキュメントディレクトリ内にあるコンテンツ管理ディレクトリへアクセス
alias zenn='cd ~/Documents/my-zenn-contents'
```

移動先に絶対パス（フルパス）を指定しているため、どのディレクトリで作業していても、`zenn`と打つだけですぐにコンテンツ管理ディレクトリへ移動します。

![コンテンツ管理ディレクトリに移動する様子](/images/alias-of-zenncli/image01.gif)

私は、Zennの記事を執筆する際に[Visual Studio Code](https://code.visualstudio.com)というエディタをZennの記事を書くときに利用しています。VSCodeの利点として、指定したディレクトリをVSCode上で瞬時に開くことのできるCodeコマンドの存在があります。Codeコマンドについては以前、Zenn記事にまとめています。
https://zenn.dev/ryuu/articles/what-vscodecommand

以下のようなコマンドで、別のディレクトリで作業していてもVSCodeでZennのコンテンツ管理ディレクトリをすぐに開いてくれます。

```shell
# Zennのコンテンツ管理ディレクトリをVSCodeで開く
$ code ~/Documents/my-zenn-contents
```

### 記事や本のプレビュー

VSCodeにはデフォルトでマークダウンのプレビュー機能が搭載されているため、マークダウンファイル（.md）をその場でプレビューしながら書くことができます。しかし、ZennにはZenn独自のマークダウン記法（メッセージやアコーディオン、トグル）があるためVSCodeのプレビュー機能では完全にプレビューできません。そこでZenn CLIには、localhostでサーバーを立ち上げてZenn上でどのように表示されるのかを投稿前に確認できるZenn Editorがあります。ホットリロードに対応しているため、ローカルで加えた変更を瞬時に反映しながら記事を書いていくことができます。

```shell
$ npx zenn preview
```

と実行し、ブラウザで[localhost:8000](http://localhost:8000)へアクセスするとZenn上での見え方をリアルタイムで確認できます。

この機能を簡単に呼び出すため、`zennpr`というエイリアスを設定しています。エイリアスを使うことで、コマンド`npx zenn preview`を簡略化でき時短に繋がります。

```shell:.zshrc
# 記事と本のプレビュー
alias zennpr='zenn && npx zenn preview --open'
```

また、`--open`オプションをつけることで、デフォルトのブラウザが自動的に開かれるようになります。予めエイリアスに`--open`オプションを含めておくことですぐにプレビューできるようにしています。ローカルで記事の修正をしたときなどすぐに見え方を確認できるのはとても便利です。

### 新しい記事・本の作成

Zenn CLIを使って新しい記事・本を作成するときのコマンド、`npx zenn new:article`を`zennna` 、`npx zenn new:book`を`zennnb`としています。記事と本とでエイリアスの後ろを（new:article , new:book）をそれぞれの頭文字にしてわかりやすくしました。記事にはスラッグ（任意の記事名）を指定でき、`npx zenn new:article`とするとランダムのスラッグにて記事が作成されます。スラッグを付ける場合はコマンドの後ろにそれぞれ、`--slug`というオプションを付けることで任意のスラッグをつけることができます。私は記事ごとに個別のスラッグをつけて管理することが多いので、`zennnas`、`zennnbs`のようにスラッグオプション（--slug）を含めたエイリアスも定義しています。

```shell:.zshrc
# 新しい記事をスラッグを指定して作成
alias zennnas='zenn && npx zenn new:article --slug'

# 新しい本をスラッグを指定して作成
alias zennnbs='zenn && npx zenn new:book --slug'
```

エイリアスに続けて、付けたいスラッグを続けるだけで簡単にオリジナルのスラッグをつけて記事を書き始められるので便利です。

```shell
$ zennnas alias-of-zenncli（任意のスラッグ）
```

スラッグ以外に、絵文字やタイトル、本の販売価格など作成時に利用できるオプションは数多くあるので、詳しくは以下の公式記事を参考にしてみてください。
https://zenn.dev/zenn/articles/zenn-cli-guide

## まとめ

今回、Zennの執筆をするときに活用しているエイリアスについて紹介しました。

Zenn CLI・ZennEditorはZennならではの機能でとても革新的なものだと感じています。この執筆体験をさらに良いものにするため、エイリアスなどで柔軟にカスタマイズすることで、更に執筆へと集中できる環境が出来上がりました。

今回紹介したエイリアスは執筆中になんども使うコマンドであるからこそエイリアスの定義で簡略化し、目的である記事の執筆に集中できる環境作りというコンセプトで考えたものになります。あくまで個人的な運用なので、ベストプラクティスとはいえませんが、あくまで参考程度に各々が使いやすいように設定して見てください。

最後まで読んでいただきありがとうございました。

## 参考

- [Zenn CLIをインストールする](https://zenn.dev/zenn/articles/install-zenn-cli)
- [Zenn CLIで記事・本を管理する方法](https://zenn.dev/zenn/articles/zenn-cli-guide)
- [ZennのMarkdown記法](https://zenn.dev/zenn/articles/markdown-guide)

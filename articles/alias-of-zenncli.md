---
title: "Zennでの執筆体験を加速するためのエイリアス設定"
emoji: "🎢"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["zenn","zenncli"]
published: false
---

# はじめに

Zenn では [ZennCLI](https://zenn.dev/zenn/articles/install-zenn-cli) と呼ばれるツールが公式から提供されており、GitHub を利用した記事の投稿、記事のバージョン管理ができます。Web ブラウザ上のエディタに縛られない柔軟な執筆環境を構築できる点が Zenn の大きな特徴です。普段、記事を執筆する中で CLI のコマンドを叩くことが多くありますが、専用のコマンドを覚えてコマンドを毎回正確に叩くプロセスを簡略化して記事の執筆に専念したいと思い、エイリアスを作成することで記事の執筆に集中できる環境を作成しました。

# 作成したエイリアス

:::message
以下のエイリアスは MacOS のデフォルトシェルである `Zsh` で利用するものになります。また記事を管理するディレクトリへのパスは環境によって各自読み替えてください。
:::

```shell:.zshrc
alias agzenn='alias | grep zenn' # ZennCLIように定義したエイリアスの確認用

alias zenn='cd ~/Documents/my-zenn-contents' # Zennの記事ディレクトリに移動
alias zennop='cd ~/Documents/my-zenn-contents && code ~/Documents/my-zenn-contents' # Zennの記事ディレクトリに移動してVSCodeで開く
alias zennopr='cd ~/Documents/my-zenn-contents && code ~/Documents/my-zenn-contents && npx zenn preview --open'
alias zennna='npx zenn new:article' # 新しい記事をランダムなスラッグで作成
alias zennnas='npx zenn new:article --slug' # 新しい記事をスラッグを指定して作成
alias zennnb='npx zenn new:book' # 新しい本をランダムなスラッグで作成
alias zennnbs='npx zenn new:book --slug ' # 新しい本をスラッグを指定して作成
alias zennpr='npx zenn preview --open' # 記事と本のプレビュー
alias zennv='npx zenn --version' # ZennCLIのバージョン確認
alias zennup='sudo npm install zenn-cli@latest' # ZennCLIのアップデート
```
# ポイント

ZennCLI をローカルにインストールしているため、基本的に npx を使ってコマンドを実行しています。ローカルリポジトリ内で実行する関係上、各エイリアスでコマンドを実行する前に `zenn` で Zenn のローカルリポジトリへ移動するようにしています。
## すぐにディレクトリを開いて執筆できるように

記事を管理しているローカルリポジトリへすぐ移動できるように `zenn` というエイリアスを設定しています。移動先に絶対パス(フルパス)を指定しているため、どのディレクトリで作業していても、`zenn`と打つだけで｀ローカルリポジトリ(my-zenn-contents)へ移動します。

私は、Zenn の記事を執筆する際に[Visual Studio Code (以下 VSCode)](https://code.visualstudio.com/)というエディタをローカルエディタとして利用しています。VSCode の利点として、指定したディレクトリを VSCode 上で瞬時に開くことのできる `Code コマンド`があります。Code コマンドについては以前、Zenn 記事にまとめています。

https://zenn.dev/ryuu/articles/what-vscodecommand

エイリアスの中の `zennop`、`zennopr`で code コマンドを利用しています。これにより、ターミナルから `zennop` とタイプするだけでどのディレクトリで作業していても VSCode で Zenn のローカルリポジトリを開いてくれます。

## 記事や本のプレビュー

VSCode にはデフォルトでマークダウンのプレビュー機能があるため、マークダウンファイル(.md)をその場でプレビューしながら書くことができます。しかし、Zenn には Zenn 独自のマークダウン記法(メッセージなど)があるため VSCode のプレビュー機能では完全にプレビューできません。そこで ZennCLI には、`localhost`でサーバーを立ち上げて Zenn 上でどのように表示されるのかを投稿前に確認できる `Zenn Editor` があります。ホットリロードに対応しているため、ローカルで加えた変更を瞬時に反映しながら記事を書いていくことができます。

```
npx zenn preview
```

と実行し、ブラウザで[localhost:8000](http://localhost:8000)へアクセスすると `ZennEditor` が起動しており、ブラウザ上での見え方をリアルタイムで確認できます。

この機能を簡単に呼び出すために、`zennpr`というエイリアスを設定しています。エイリアスを使うことで、プレビューを開くときのコマンドを `npx zenn preview` を簡略化でき時短に繋がります。
エイリアスの中で、プレビューコマンドに加えてオプションを追加しているところがポイントになります 。

```
alias zennpr='npx zenn preview --open' # 記事と本のプレビュー
```
公式ドキュメントには詳しく記載がありませんが、ここで `--open` というオプションをつけることで、デフォルトのブラウザでプレビュー画面が自動的に開かれるようになります。予めエイリアスに--open オプションを含めておくことですぐにプレビューできるようにしています。ローカルで記事の修正をしたときなどすぐに見え方を確認できるのはとても便利です。

## 新しい記事・本の作成

ZennCLI を使って新しい記事・本を作成するときのコマンド、`npx zenn new:article`を `zennna` 、`npx zenn new:book`を `zennnb` としています。記事と本とでエイリアスの後ろを(new:article , new:book)をそれぞれの頭文字にしてわかりやすくしました。記事にはスラッグ(任意の記事名)を指定でき、`npx zenn new:article`とするとランダムのスラッグにて記事が作成されます。スラッグを付ける場合はコマンドの後ろにそれぞれ、`--slug`というオプションを付けることで任意のスラッグをつけることができます。私は記事ごとに個別のスラッグをつけて管理することが多いので、`zennnas`、`zennnab`といった形でスラッグオプション(--slug)を含めたエイリアスも定義しています。エイリアス似続けて、スラッグをつづけるだけで簡単にオリジナルのスラッグをつけて記事を作れるので便利です。スラッグ以外に、絵文字やタイトル、本の販売価格など作成時に利用できるオプションは多くあるので、詳しくは以下の公式記事を参考にしてみてください。

https://zenn.dev/zenn/articles/zenn-cli-guide

# まとめ

今回、Zenn の執筆をするときに活用しているエイリアスについて紹介しました。

ZennCLI、ZennEditor は Zenn ならではの機能でとても革新的なものだと感じています。この執筆体験をさらに良いものにするため、エイリアスなどで柔軟にカスタマイズすることで、更に執筆へと集中できる環境が出来上がりました。

今回紹介したエイリアスは執筆中になんども使うコマンドであるからこそエイリアスの定義で簡略化し、目的である記事の執筆に集中できる環境作りというコンセプトで考えたものになります。あくまで個人的な運用なので、ベストプラクティスとはいえませんが、あくまで参考程度に各々が使いやすいように設定して見てください。

最後まで読んでいただきありがとうございました。
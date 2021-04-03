---
title: "Zennでの執筆体験を加速するためのエイリアス設定"
emoji: "🎢"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["zenn","zenncli"]
published: false
---

# はじめに

Zenn には [ZennCLI](https://zenn.dev/zenn/articles/install-zenn-cli) と呼ばれるツールが公式から提供されており、GitHub を利用したリポジトリ単位での記事の管理ができます。これにより、Web ブラウザ上に縛られない柔軟な執筆環境を構築できるため重宝しております。普段、記事を執筆する中で CLI のコマンドを叩くことが多くありますが、専用のコマンドを覚えてコマンドを毎回正確に叩くプロセスを簡略化して記事の執筆に専念したいと思い、エイリアスを作成することで記事の執筆に集中できる環境を作成しました。

# 作成したエイリアス

:::message
作成したエイリアスは MacOS のデフォルトシェルである `Zsh` で利用するものになります。また記事を管理するディレクトリへのパスは環境によって各自読み替えてください。
:::

```shell:.zshrc
alias agzenn='alias | grep zenn' # ZennCLIエイリアスの確認用

alias zenn='cd ~/Documents/my-zenn-contents' # Zennの記事ディレクトリに移動
alias zennop='cd ~/Documents/my-zenn-contents && code ~/Documents/my-zenn-contents' # Zennの記事ディレクトリに移動してVSCodeで開く
alias zennopr='cd ~/Documents/my-zenn-contents && code ~/Documents/my-zenn-contents && npx zenn preview --open'
alias zennna='npx zenn new:article' # 新しい記事をランダムなスラッグで作成
alias zennnas='npx zenn new:article --slug' # 新しい記事をスラッグを指定して作成
alias zennnb='npx zenn new:book' # 新しい本をランダムなスラッグで作成
alias zennnbs='npx zenn new:book --slug ' # 新しい本をスラッグを指定して作成
alias zennpr='npx zenn preview' # 記事と本のプレビュー
alias zennv='npx zenn --version' # ZennCLIのバージョン確認
alias zennup='sudo npm install zenn-cli@latest' # ZennCLIのアップデート
```
# ポイント

# すぐにディレクトリを開いて執筆できるように

記事を管理しているローカルリポジトリへすぐ移動できるように `zenn` というエイリアスを設定しています。移動先に絶対パス(フルパス)を指定しているため、どのディレクトリで作業していても、`zenn`と打つだけで｀ローカルリポジトリ(my-zenn-contents)へ移動します。

私は、Zenn の記事を執筆する際に[Visual Studio Code (以下 VSCode)](https://code.visualstudio.com/)というエディタをローカルエディタとして利用しています。VSCode の利点として、指定したディレクトリを VSCode 上で瞬時に開くことのできる `Code コマンド`があります。Code コマンドについては以前、Zenn 記事にまとめています。

https://zenn.dev/ryuu/articles/what-vscodecommand

エイリアスの中の `zennop`、`zennopr`で code コマンドを利用しています。これにより、ターミナルから `zennop` とタイプするだけでどのディレクトリで作業していても VSCode で Zenn のローカルリポジトリを開いてくれます。

# 記事や本のプレビュー

VSCode を使ってマークダウンファイル(.md)を書くときに、VSCode のプレビュー機能を使ってプレビューできます。しかし、Zenn には Zenn 独自の書き方(メッセージなど)があるため VSCode のプレビュー機能では完全に再現できません。そこで ZennCLI には、`localhost`でサーバーを立ち上げて Zenn 上でどのように表示されるのかを投稿前に確認できる機能があります。ホットリロードに対応しているため、ローカルで加えた変更を瞬時に反映しながら記事を書いていくことができます。この機能をエイリアスを使って呼び出すために、`zennpr`というエイリアスを設定しています。エイリアスを使うことで、プレビューを開くときのコマンドを `npx zenn preview` を簡略化でき時短に繋がります。



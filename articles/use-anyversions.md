---
title: "オールインワンな開発環境をanyenvを使って構築する"
emoji: "🧭"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["anyenv"]
published: false
---

# はじめに

各プログラミング言語で開発を進める際には必ずそれぞれの言語での `開発環境` を構築することから始まります。しかし、プログラミング言語にはバージョンが多く存在するため、バージョンによって動かないパッケージが存在したり、プロジェクト毎に異なるバージョンを使いたいといったニーズが存在します。またプログラミング言語がバージョンアップするたびに開発環境の構築方法が変わったりすることも多くあるため、複数のバージョンを使用するときに苦労を強いられることもあります。

そこで各プログラミング言語向けにバージョンをよしなに切り替えることのできる便利なツールが存在します。有名なツールとして Ruby なら [rbenv](https://github.com/rbenv/rbenv)，Node.js なら [nodenv](https://github.com/nodenv/nodenv) があります。

これらのツールはそれぞれ GitHub からのクローン、Homebrew などのパッケージ管理ツールを使いインストールが可能です。バージョンをローカル(プロジェクト単位)、グローバル(システム単位)で柔軟に切り替えられるため、どれも人気のツールとなっています。

昨今、オールインワンでかつ柔軟なツールが多く出る中、この記事のタイトルにもある「anyenv」は管理公式が「All in one for **env」と謳っているとおり、有名なバージョン管理ツールをひとまとめにしたオールインワンのラッパーです。つまり rbenv や nodenv の管理ツールといった立ち位置といえます。今回はそんな `オールインワン` なツール「anyenv」を紹介します。

anyenv の Git リポジトリはこちら
https://github.com/anyenv/anyenv

# anyenvでインストールできるもの

anyenv 1.1.2-1 時点では以下のようになっています。

| env 系ツール名 | 対象 |
| ---- | ---- |
| Renv | R  |
| crenv | Crystal |
| denv | D |
| erlenv | Erlang |
| exenv | Elixir |
| goenv | Go |
| hsenv | Haskell |
| jenv | Java |
| jlenv | Text |
| luaenv | Lua |
| nodenv | Node.js |
| phpenv | PHP |
| plenv | Perl |
| pyenv | Python |
| rbenv | Ruby |
| sbtenv | sbt |
| scalaenv | Scala |
| swiftenv | Swift |
| tfenv | Terraform |

# インストール

## 手動セットアップ

Git からチェックアウトしてインストールする方法です。OS を選ぶことなくインストールできます。

ます、Git を使い anyenv の Github リポジトリからクローンしてきます。Git コマンドを使うため、事前に Git コマンドがインストールされている必要があります。
以下のコマンドを実行してバージョンがうまく返ってくれば Git コマンドがインストールされています。

```shell
$ git --version

git version 2.31.0
```

Git が使えることを確認したら以下のコマンドでユーザーのホームディレクトリに anyenv 本体をクローンします。

```shell
$ git clone https://github.com/anyenv/anyenv ~/.anyenv
```

クローンが終わると、ユーザーのホームディレクトリに `.anyenv` が作られます。

```shell:~/
$ ls -a

. .. .anyenv
```

クローンしただけでは、パスが通っていないためパスを通します。
WSL で Ubuntu を使用している場合は以下のコマンドを実行。

```shell
$ echo 'export PATH="$HOME/.anyenv/bin:$PATH"' >> ~/.bash_profile
```

Mac の場合は以下のコマンドを実行します。

```shell
$ echo 'export PATH="$HOME/.anyenv/bin:$PATH"' >> ~/.zshrc
```

コマンドを実行したら設定を有効化するためシェルに再ログインします。以下のコマンドで再ログインできます。

```shell
$ exec $SHELL -l
```

つづいて、anyenv の初期化をしていきます。ターミナルで以下のコマンドを実行。

```shell
$ anyenv init

# Load anyenv automatically by adding
# the following to ~/.bash_profile:

eval "$(anyenv init -)"
```

コマンドを実行すると設定の手順が表示されます。指示に従い、設定ファイル(この場合は.bash_profile)へ初期化処理を記述し初期設定を完了させます。

```shell
$ echo 'eval "$(anyenv init -)"' >> ~/.bash_profile
```

コマンド実行後またシェルに再ログインします。

続けて複数の env をインストールするために必要なセットアップをしていきます。

```shell
anyenv install --init
```

コマンドを実行途中で確認を求められたら `y` と入力し、続行します。`Completed!`と表示されれば完了です。
これで手動での anyenv セットアップは終了です。

## Homebrewを使ったインストール

MacOS など、パッケージマネージャとして Homebrew を使用している場合、簡単にインストールして使い始められます。
brew コマンドが使える状況で以下のコマンドを実行します。

```shell
$ brew install anyenv
```

:::message
Homebrew でインストールした場合はすでにパスが通っているため、Git からクローンする場合と異なり手動でパスを通す作業はありません。
:::

インストールが完了したら初期化処理をします。

```shell
$ anyenv init
```

設定ファイルに初期化処理を記述するように促されるので、`.zshrc`へ記述します。

```
$ echo 'eval "$(anyenv init -)"' >>  ~/.zshrc
```

これで初期化ができました。以下のコマンドで再ログインします。

```shell
$ exec $SHELL -l
```

続けて anyenv のセットアップをします。

```shell
$ anyenv install init
```

コマンドを実行途中で確認を求められたら `y` と入力し、続行します。`Completed!`と表示されれば完了です。

# インストール可能なenv系を一覧表示

以下のコマンドで現在の anyenv のバージョンで使用できる env 系ツールを一覧表示できます。

```shell
$ anyenv install -l

  Renv
  crenv
  denv
  erlenv
  exenv
  goenv
  hsenv
  jenv
  jlenv
  luaenv
  nodenv
  phpenv
  plenv
  pyenv
  rbenv
  sbtenv
  scalaenv
  swiftenv
  tfenv
```

インストール可能なツールが表示されました。ここから必要な env 系ツールをインストールできます。

# env系ツールのインストール

例として Node.js のバージョンマネージャ `nodenv` をインストールしてみます。
まず nodenv を anyenv 経由でインストールします。

```shell
$ anyenv install nodenv

...

Install nodenv succeeded!
Please reload your profile (exec $SHELL -l) or open a new session.
```

インストールが成功すると設定の再読み込みを求められるため、指示に従い `exec $SHELL -l` でシェルへ再ログインします。

再ログインするとインストールした env 系ツールのコマンドが使用可能になります。

インストールしたツールは、anyenv と同様に、以下のようなコマンドが使用できます。例では、上に続けて `nodenv` をインストールした場合で説明しています。
env 系ツールでほとんど操作は変わらないため、適宜先頭を置き換えてください。

## インストールできるバージョンの表示

```shell
$ nodenv install -l
```

## バージョンを指定してのインストール

```shell
# Node.js 14.15.4(LTS)をインストール
$ nodenv install 14.15.4

# nodevをrehash
$ nodenv rehash

# インストールされたバージョンを確認
$ nodenv versions
  14.15.4
```
## システム全体で使うバージョンを指定
インストールしただけではまだ node コマンドを使用できないため、システム上でメインとして使うバージョンを指定する必要があります。

```shell
$ nodenv global 14.15.4

# Node.jsのバージョンを確認
$ node -v
  node 14.15.4
```

## 複数バージョンの切り替え

env 系ツールを使う醍醐味である複数バージョンの切り替えです。
ここでは新しい別のバージョンを新たにインストールして切り替える例を紹介します。

```shell
# 異なるバージョン(15.10.0)をインストール
$ nodenv install 15.10.0

~
Installed node-v15.10.0-linux-x64 to /home/user/.anyenv/envs/nodenv/versions/15.10.0

# 15.10.0がインストールされた グローバルで指定されている14.15.4は * で示される
$ nodenv versions
* 14.15.4 (set by /home/user/.anyenv/envs/nodenv/version)
  15.10.0

# システムで使うバージョンを15.10.0に変更
$ nodenv global 15.10.0

# 切り替わったか確認
$ node -v
  15.10.0

# システム全体で使うバージョンが15.10.0に変更されている
$ nodenv versions
  14.15.4
* 15.10.0 (set by /home/user/.anyenv/envs/nodenv/version)
```

## プロジェクトごとに使いたいバージョンを切り替える

先程設定した(15.10.0)と **異なるバージョン(14.15.4)** をプロジェクト内で使いたいという前提でバージョンを切り替える例を紹介します。

```shell
# システムでのバージョンを確認
$ node -v
  15.10.0

# バージョンを指定したいプロジェクトへ移動
$ cd ~/Documents/example-project

# 使用したいバージョンを(local)で指定
~/Documents/example-project $ nodenv local 14.15.4

# Node.jsのバージョンを確認
~/Documents/example-project $ node -v
  14.15.4

# バージョンを指定するとプロジェクト内に .node-version ファイルが作成される
```

上の流れでバージョンを切り替えることができます。anyenv を経由して柔軟に env 系のツールをインストール、バージョンの指定までできるのは便利です。

# おわりに

今回は、オールインワンなバージョンマネージャ「anyenv」についてまとめました。これ１つでバージョン管理はバッチリというぐらいに柔軟にツールの管理ができてとても便利だと感じました。anyenv を使うことでプログラミング環境に合わせて PHP なら composer といったおなじみのパッケージマネージャも合わせて導入されるたりと痒いところに手が届くの便利です。プログラミング初学者の誰もが通るプログラミング環境構築という壁が anyenv によって解決できそうな気がしました。開発環境に時間をかけることなく開発の方に集中したほうが望ましいため、このようなツールは積極的に活用していきたいところですね。

最後まで読んでいただきありがとうございました。
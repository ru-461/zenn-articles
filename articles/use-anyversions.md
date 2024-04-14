---
title: "オールインワンな開発環境をanyenvで構築する"
emoji: "🧭"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["anyenv", "nodenv", "環境構築", "初心者"]
published: true
---

## はじめに

世の中には数多くのプログラミング言語が存在し、必ず開発環境を構築することは避けて通れません。プログラミングの環境構築中によくある問題として、特定のバージョンによって動かないパッケージの存在など、**プロジェクト毎に異なるバージョンを使いたいといったケースが存在します**。またプログラミング言語がバージョンアップするたびに開発環境の構築方法が変わったりすることも多くあるため、複数のバージョンを使用するときに苦労を強いられることもあります。

各言語向けのツールとしてRubyなら[rbenv](https://github.com/rbenv/rbenv)，Node.jsなら[nodenv](https://github.com/nodenv/nodenv)といった環境管理**env系**ツールがあります。

これらのツールはそれぞれGitHubからのクローン、Homebrewなどのパッケージ管理ツールを使いインストールが可能です。使用するバージョンを場面に応じて柔軟に切り替えられるため、どれも人気のツールとなっています。

タイトルにもある「anyenv」は、公式が「All in one for env」と謳っている、オールインワンのバージョンマネージャです。今回はそんなオールインワンなツール「anyenv」を紹介します。

anyenvのGitHubリポジトリはこちらになります。
https://github.com/anyenv/anyenv

## anyenvでインストールできるもの

v1.1.5時点では以下のツールをanyenv経由でインストール可能です。

| ツール | 対象環境・言語 |
| ---- | ---- |
| Renv | R  |
| crenv | Crystal |
| denv | D |
| erlenv | Erlang |
| exenv | Elixir |
| goenv | Go |
| hsenv | Haskell |
| jenv | Java |
| jlenv | Julia |
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

## インストール

### 実行環境

- Ubuntu 22.04 LTS (WSL2)
- macOS Sonoma 14.4.1

### 手動インストール

Gitからチェックアウトしてインストールする方法です。Gitを使うため、特定のパッケージマネージャや、OSを選ぶことなくインストールできます。

まず、Gitを使いanyenvのGitHubリポジトリから本体をクローンしてきます。Gitコマンドを使うため、**事前に Git コマンドがインストールされている必要**があります。
以下のコマンドを実行してバージョン情報がうまく返ってくればGitコマンドがインストールされています。

```shell
# Gitのバージョンを確認
$ git --version
git version 2.34.1
```

Gitが使えることを確認したら以下のコマンドでユーザーのホームディレクトリにanyenv本体をクローンします。

```shell
$ git clone https://github.com/anyenv/anyenv ~/.anyenv
```

クローンが終わると、ユーザーのホームディレクトリに`.anyenv`が作られます。

```shell
$ ls -a

. .. .anyenv
```

クローンしただけでは、パスが通っていないためパスを通します。
シェルにbashを利用している場合は以下のコマンドを実行。

```shell:bash
$ echo 'export PATH="$HOME/.anyenv/bin:$PATH"' >> ~/.bash_profile
```

zshを使用している場合は以下のコマンドを実行します。

```shell:zsh
$ echo 'export PATH="$HOME/.anyenv/bin:$PATH"' >> ~/.zshrc
```

コマンドを実行したら設定を有効化するためシェルに再ログインします。以下のコマンドで再ログインできます。

```shell
# シェルを再起動
$ exec $SHELL -l
```

つづいて、anyenvの初期化をしていきます。ターミナルで以下のコマンドを実行。

```shell
$ anyenv init

# Load anyenv automatically by adding
# the following to ~/.bash_profile:

eval "$(anyenv init -)"
```

コマンドを実行すると設定の手順が表示されます。指示に従い、設定ファイル（この場合は.bash_profile）へ初期化処理を記述し初期設定を完了させます。

```shell
$ echo 'eval "$(anyenv init -)"' >> ~/.bash_profile
```

コマンド実行後またシェルに再ログインします。

続けて複数のenvをインストールするために必要なセットアップをしていきます。

```shell
$ anyenv install --init
Manifest directory doesn't exist: /root/.config/anyenv/anyenv-install
Do you want to checkout https://github.com/anyenv/anyenv-install.git? [y/N]:
```

`Do you want to checkout...`と確認されるので`y`で続行します。`Completed!`と表示されれば完了です。これでanyenvのセットアップは終了です。

### Homebrewを使ったインストール

macOSなど、パッケージマネージャとしてHomebrewを使用している場合、簡単にインストールして使い始められます。

brewコマンドが使える状況で以下のコマンドを実行します。

```shell
$ brew install anyenv
```

:::message
Homebrewでインストールした場合はすでにパスが通っているため、Gitからクローンする場合と異なり手動でパスを通す作業はありません。
:::

インストールが完了したら初期化処理をします。

```shell
$ anyenv init
```

設定ファイルに初期化処理を記述するように促されるので、`.zshrc`へ記述します。

```shell:.zshrc
$ echo 'eval "$(anyenv init -)"' >> ~/.zshrc
```

これで初期化ができました。以下のコマンドで再ログインします。

```shell
# シェルを再起動
$ exec $SHELL -l
```

続けてanyenvのセットアップをします。

```shell
$ anyenv install --init
```

コマンドを実行途中で確認を求められたら続行します。`Completed!`と表示されれば完了です。

## インストール可能なenv系を一覧表示

以下のコマンドで現在のanyenvのバージョンで使用できるenv系ツールを一覧表示できます。

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

現在インストール可能なenv系ツールが表示されました。ここから必要なenv系ツールをインストールできます。

## env系ツールのインストール

例としてNode.jsのバージョンマネージャnodenvをインストールしてみます。
まずnodenvをanyenv経由でインストールします。

```shell
$ anyenv install nodenv
...

Install nodenv succeeded!
Please reload your profile (exec $SHELL -l) or open a new session.
```

インストールが成功すると設定の再読み込みを求められるため、指示に従い`exec $SHELL -l`でシェルへ再ログインします。

再ログインするとインストールしたenv系ツールのコマンドが使用可能になります。

インストールしたツールは、anyenvと同様に、以下のようなコマンドが使用できます。例では、上に続けてnodenvをインストールした場合で説明しています。
env系ツールでほとんど操作は変わらないため、適宜先頭を置き換えてください。

### インストールできるバージョンの表示

```shell
$ nodenv install -l
  ~
  # インストール可能なすべてのバージョンが表示される
```

### バージョンを指定してのインストール

```shell
# Node.js 20.12.2(LTS)をインストール
$ nodenv install 20.12.2

# インストールされているバージョンを一覧表示
$ nodenv versions
  20.12.2
```

### システム全体で使うバージョンを指定

インストールしただけではまだNode.jsは使用できないため、システム上でメインとして使うバージョンを明示的にセットしてあげる必要があります。

```shell
# システム全体で使用するバージョンを指定
$ nodenv global 20.12.2

# バージョンを確認
$ node --version
v20.12.2
```

### 複数バージョンの切り替え

env系ツールを使う醍醐味である複数バージョンの切り替えです。
ここでは新しい別のバージョンを新たにインストールして切り替える例を紹介します。

```shell
# 異なるバージョン(21.7.3)をインストール
$ nodenv install 21.7.3

~
Installed node-v21.7.3-linux-x64 to /home/user/.anyenv/envs/nodenv/versions/21.7.3

# バージョン21.7.3のインストールが完了
# システム全体で適用されている20.12.2は先頭に '*' がつく
$ nodenv versions
* 20.12.2 (set by /home/user/.anyenv/envs/nodenv/version)
  21.7.3

# システムで使うバージョンを21.7.3に変更
$ nodenv global 21.7.3

# 切り替わったか確認
$ node -version
  21.7.3

# システム全体で使うバージョンが21.7.3に変更されている
$ nodenv versions
  20.12.2
* 21.7.3 (set by /home/user/.anyenv/envs/nodenv/version)
```

### プロジェクトごとに使いたいバージョンを切り替える

先程設定したバージョンと **異なるバージョン**をプロジェクト内で使いたいという前提でバージョンを切り替える例を紹介します。

Node.js 21.7.3を20.12.2に切り替えてみます。

```shell
# 現在適用されているバージョンを確認
$ node --version
  21.7.3

# 異なるバージョンを使用したいプロジェクトへ移動
$ cd ~/Documents/example-project

# プロジェクト内で使用したいバージョンを指定
$ nodenv local 20.12.2

# バージョンを確認
$ node --version
  20.12.2

# バージョンを指定するとプロジェクト内に .node-version ファイルが作成される
```

上の流れでバージョンを切り替えることができます。

なお、上記で設定したローカルバージョンはプロジェクト内のみで有効となる点に注意が必要です。
ディレクトリを移動すると`global`で設定したバージョンが優先的に使用されます。

```shell
# ホームディレクトリへ移動
$ cd

# システム全体で使うバージョンが表示される
$ node --version
  21.7.3
```

## Node.js以外のenv系ツールを使用する

anyenvでは例で紹介したnodenv以外にも、多言語のenv系ツールをまとめて管理できます。以下のコマンドでanyenvにて管理しているenv系ツールを表示します。

```shell
$ anyenv versions

nodenv:
  system
* 20.12.2 (set by /home/user/.anyenv/envs/nodenv/version)
phpenv:
  7.4.15
rbenv:
* 2.7.2 (set by /home/user/.anyenv/envs/rbenv/version)
  3.0.0
```

## env系ツールの一括アップデート

ここまで、env軽ツールのインストールとバージョン管理について紹介してきました。

anyenvで管理できるenv系ツールはよくアップデートでバージョンが上がります。複数のenvを使用している場合、アップデートを確認して場合それぞれ個別にアップデートするのは大変です。そこでenv系のバージョンをまとめてアップデートできる便利なプラグインがあるため紹介します。

プラグインのリポジトリはこちらになります。
https://github.com/znz/anyenv-update

```shell
# .anyenv 配下にプラグイン管理用のディレクトリ作成
$ mkdir -p $(anyenv root)/plugins

# プラグインをGitHubからクローン
$ git clone https://github.com/znz/anyenv-update.git $(anyenv root)/plugins/anyenv-update

# インストールされたenvの一括アップデート
$ anyenv update
  ...
  Updating 'anyenv'...
  Updating 'anyenv/anyenv-update'...
  Updating 'nodenv'...
  Updating 'nodenv/node-build'...
  Updating 'nodenv/nodenv-vars'...
  Updating 'anyenv manifest directory'...
```

これでバージョンアップがあったときにも`anyenv update`とひとつのコマンドで対応できるようになり便利です。

## おわりに

今回は、オールインワンなバージョンマネージャ「anyenv」についてまとめました。これ１つでバージョン管理はバッチリというぐらいに柔軟にツールの管理ができてとても便利だと感じました。anyenvを使うことでcomposerやbundlerといったおなじみのパッケージマネージャも合わせて導入されたりと痒いところに手が届くの便利です。**anyenvを使うことで誰もが通るプログラミング環境構築という大きな壁も柔軟に解決できそうな気がしました**。開発環境に時間をかけることなく開発の方に集中したほうが望ましいため、このようなツールは積極的に活用していきたいところですね。

最後まで読んでいただきありがとうございました。

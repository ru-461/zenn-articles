---
title: "今更ながら 重い腰を上げて WSL2 へ Homebrew をインストールした"
emoji: "⛱"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["wsl2","windows","homebrew"]
published: false
---

# はじめに

[前回の記事](https://zenn.dev/ryuu/articles/brew-yarn-warning)に続き Homebrew 関連の記事になります。

最近、Windows の WSL2 と M1 Macbook の環境を行き来することが多く、それぞれの環境でパッケージマネージャーを使いパッケージの依存関係を管理していました。WSL2 ではデフォルトで使用できる `apt` 、Mac では最近 M1 チップに対応した `Homebrew` というパッケージマネージャーをそれぞれ使用しています。

それぞれのパッケージマネージャで使用するコマンドが異なるため、使い勝手が微妙に異なることがあり、コマンドを暗記していても瞬間的に出てこないことがありました。そこで WSL2 にも homebrew が対応しているという情報を目にして今更ながら WSL2 に homebrew を導入しました。

Homebrew といえば MacOS ユーザー の特権という先入観で Homebrew は Mac OS だけで使えるパッケージマネージャーかとずっと思っていたのですが、公式サイトを確認すると。

> The Missing Package Manager for macOS (or Linux)
(macOS（またはLinux）用パッケージマネージャー)

と謳われており Linux もサポートしていることに驚きました。

これは過去に Linuxbrew といった名前で開発されていたパッケージマネージャーがあったことに関係するようです。2019 年 2 月に公開された [Homebrew 2.0.0](https://brew.sh/2019/02/02/homebrew-2.0.0/) より Linux と WSL(Windows Subsystem for Linux)の公式サポートが行われ、現在 Homebrew というパッケージマネージャーに統合される形で使用できるようです。

今までずっと apt でパッケージ管理をしてきました。通常ユーザーで作業する際に apt だと管理者権限(sudo コマンド)必須というのが少し気になっていました。Homebrew ではホームディレクトリ内でパッケージを管理するため sudo コマンドを使うことがないというのも魅力的です。また、Mac で brew を使っていることもあり、使用するシェルやパッケージマネージャーは異なる環境でもコマンドが共通して使えるという点にとても魅力を感じました。パッケージマネージャーを乗り換えるのは少し抵抗がありますが重い腰を上げてこの機会に Homebrew へ乗り換えました。

M1 Mac 登場のときも話題になりましたが、Homebrew は CPU のアーキテクチャ(x86_64 や ARM)によってインストール方法が微妙に異なります。今回は私の環境(x86_64)に合わせて紹介します。

Homebrew の公式サイト(日本語)はこちらです。
https://brew.sh/index_ja

# 環境

- CPU Core i5 9400F(x64_86)
- Windows10 バージョン 20H2
- WSL2 (Ubuntu 20.04.1 LTS (Focal Fossa))

# Homebrewのインストール
## インストールの準備

Homebrwe を導入するにあたりパッケージを最新に更新しておきます。

```shell
$ sudo apt update
```

また Homebrew を導入するのに以下の条件を満たす必要があります。(公式ドキュメント参照)

- GCC 4.7.0 以降
- Linux 2.6.32 以上
- Glibc 2.13 以上
- 64-bit x86_64 の CPU

現在使っている PC では x86_64 の CPU 上で WSL2 (Ubuntu 20.0.4) が動作しているため、以下のパッケージをコマンドでインストールします。

```shell
$ sudo apt-get install build-essential curl file git
```

:::message
このときにインストールしている build-essential とは開発のビルドパッケージをまとめてインストールできるもののようです
:::

## インストールスクリプトの実行

必要条件を満たしたらターミナル上で以下のコマンドを実行します。

このコマンドは Homebrew のトップページに表示されているものになります
![インストールスクリプトの画像](https://storage.googleapis.com/zenn-user-upload/pk2evmbol40spx5no4fi85gi4lqn)
*Macとlinuxで同じコマンドを使用する*

:::message
インストールに使用するコマンドは今後変更されることも予測されるのでその都度公式ページから最新の情報を参照するようにしてください。
:::

WSL2 のターミナル上で以下のコマンドを実行します。

```shell
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

コマンドを実行するとインストールされるスクリプトと、インストールで作成されるディレクトリが一覧で表示されます。
ここからは Enter で対話形式で進めていきます。

インストールと同時に初期設定をするため完了までしばらく時間がかかります。

ターミナルに `==> Installation successful!` と表示されれるとインストールが完了です。

しかしまだ PATH 関連で Warning が表示されており、`brewコマンド`を使うことができません。
ターミナルに表示された Next steps と公式ドキュメントを参考に手動で初期設定をしていきます。

# Homebrewの初期設定

## パスを通してコマンドを使えるようにする

コマンドを叩けるように現在使用しているシェルのプロファイルにパスを追記する必要があります。
ターミナルに表示されている指示どおりに公式ドキュメントから以下のコマンドを順番に実行します。

```shell
$ test -d ~/.linuxbrew && eval $(~/.linuxbrew/bin/brew shellenv)
$ test -d /home/linuxbrew/.linuxbrew && eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)
$ test -r ~/.bash_profile && echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.bash_profile
$ echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.profile
```

上手く反映させるために一旦シェルを再起動します。

```shell
$ exec $SHELL -l
```

これでパスの追加が完了です。
試しに brew と打って以下のようなコマンド候補が返ってくればパスが上手く通っています。

```shell
$ brew

Example usage:
  brew search [TEXT|/REGEX/]
  brew info [FORMULA...]
  brew install FORMULA...
  ...
```

これで導入は完了です。お疲れ様でした。

brew コマンド実行の際に怒られる場合はおなじみの `brew doctor` で細かく指摘を受けることができます。

Homebrew ではパッケージを Fomula と呼び、以下のようにインストールします。

```shell
# 有名な treeコマンドを Homebrew でインストール
$ brew install tree
```
インストールが完了すると同時にパスも通るためターミナルからすぐ実行できます。

インストールされた場所の確認。

```shell
$ which tree

/home/linuxbrew/.linuxbrew/bin/tree
```

WSL２ではパッケージを全て `/home/linuxbrew/.linuxbrew` の配下で管理するようです。

# さいごに

今回、WSL2 の環境にパッケージマネージャー Homebrew をインストールしてみました。
インターネットの情報ではパッケージインストールを brew コマンドで解説している情報も多く、Ubuntu だと apt コマンドで置き換えてインストールするなど少し手間でした。Homebrew が使えるようになったことで MacOS と同じ感覚で操作できる、コマンドの管理、パッケージの管理がしやすいなどの大きなメリットがありました。

なにより sudo を使わずに手軽に使えるのが大きいです。Homebrew を思い切って導入したことで WSL2 での開発が捗りそうです。

最後まで読んでいただきありがとうございました。
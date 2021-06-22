---
title: "今更ながら重い腰を上げて WSL2 へ Homebrew をインストールした"
emoji: "🧳"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["wsl", "ubuntu", "windows", "homebrew"]
published: true
---

# はじめに

[前回の記事](https://zenn.dev/ryuu/articles/brew-yarn-warning)に続き Homebrew 関連の記事になります。

最近、Windows の WSL2 と M1 Macbook の環境を行き来することが多く、それぞれの環境でパッケージマネージャーを使いパッケージの依存関係を管理していました。WSL2 ではデフォルトで使用できる `apt` 、Mac では[3.0.0](https://brew.sh/2021/02/05/homebrew-3.0.0/) にて最近 M1 チップに対応した `Homebrew` というパッケージマネージャーをそれぞれ使用しています。

それぞれのパッケージマネージャで使用するコマンドが異なるため、環境が変わったときにコマンドを暗記していても瞬間的に出てこないことがありました。そこで WSL2 にも Homebrew が対応しているという情報を目にして今更ながら WSL2 に Homebrew を導入しました。

Homebrew といえば macOS ユーザー の特権という先入観で Homebrew は mac OS だけで使えるパッケージマネージャーかとずっと思っていたのですが、公式サイトを確認すると。

> The Missing Package Manager for macOS (or Linux)
(macOS（または Linux）用パッケージマネージャー)

と謳われており Linux もサポートしていることに驚きました。

これは過去に Linuxbrew といった名前で開発されていたパッケージマネージャーがあったことに関係するようです。2019 年 2 月に公開されたバージョン [2.0.0](https://brew.sh/2019/02/02/homebrew-2.0.0/) より Linux と WSL(Windows Subsystem for Linux)の公式サポートが行われ、現在 Homebrew がサポートするといった形で使用できるようです。

今までずっと apt を使ってパッケージ管理をしてきました。通常ユーザーで作業する際に apt だと管理者権限(sudo コマンド)必須というのが少し気になっていました。**Homebrew ではホームディレクトリ内でパッケージを管理するため sudo を使わない**というのも魅力的です。また、Mac で Homebrew を使っていることもあり、それぞれのプラットフォームで**コマンドが共通して使える**という点にとても魅力を感じました。パッケージマネージャーを乗り換えるのは少し抵抗がありますが重い腰を上げてこの機会に apt から Homebrew へ乗り換えました。

:::message
Apple Silicon Mac 登場のときも話題になりましたが、Homebrew は CPU のアーキテクチャ(x86_64 や ARM)によってインストール方法や使用できるパッケージが異なります。今回は私の環境(x86_64)に合わせて紹介します。
:::

Homebrew の公式サイト(日本語)はこちらです。

https://brew.sh/index_ja

# 環境

- CPU Core™ i5-9400F (x86_64)
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
このときにインストールしている `build-essential` とは開発のビルドパッケージをまとめてインストールできるもののようです
:::

## インストールスクリプトの実行

必要条件を満たしたらターミナル上で以下のワンライナーを実行します。

これは Homebrew のトップページに表示されているワンライナーになります
![インストールワンライナーの画像](https://storage.googleapis.com/zenn-user-upload/pk2evmbol40spx5no4fi85gi4lqn)
*Macとlinux共通で使用する*

:::message
インストールに使用するワンライナーは今後変更されることも予測されるのでその都度公式ページから最新の情報を参照するようにしてください。
:::

WSL2 のターミナル上で**一般ユーザーに切り替えたうえで実行します。**

```shell
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

コマンドを実行するとインストールされるスクリプトと、インストールで作成されるディレクトリが一覧で表示されます。
ここからは Enter で対話形式で進めていきます。

インストールと同時に初期設定をするため完了までしばらく時間がかかります。

ターミナルに `==> Installation successful!` と表示されれるとインストールが完了です。

しかしまだ PATH 関連で Warning が表示されており、`brewコマンド` を使うことができません。
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

設定を上手く反映させるためにシェルを再起動します。

```shell
# シェル再起動
$ exec $SHELL -l
```

これでパスの追加が完了です。
試しに `brew` と打って以下のようなコマンド候補が返ってくればパスが上手く通っています。

```shell
$ brew

Example usage:
  brew search [TEXT|/REGEX/]
  brew info [FORMULA...]
  brew install FORMULA...

  ...
```

導入されたバージョンは以下のコマンドで確認できます。

```shell
$ brew --version

Homebrew 3.0.2
Homebrew/linuxbrew-core (git revision b2e4cd; last commit 2021-02-27)
```

これで導入は完了です。お疲れ様でした。

brew コマンド実行の際に怒られる場合はおなじみの `brew doctor` で細かく指摘を受けることができます。

Homebrew ではパッケージを Fomula と呼び、以下のようにインストールします。

```shell
# 有名な treeコマンドを Homebrew を使ってインストール
$ brew install tree
```
インストールが完了すると同時にパスも通るためターミナルからすぐに実行できます。

インストールされた場所の確認。

```shell
$ which tree

/home/linuxbrew/.linuxbrew/bin/tree
```

パスを見て分かるように、WSL にインストールした Homebrew ではパッケージを `/home/linuxbrew/.linuxbrew` の配下で管理します。
ホームディレクトリ配下で完結するため、**システム環境をあまり汚さない**のがいいですね。

# さいごに

今回、WSL2 の環境にパッケージマネージャーである Homebrew をインストールしてみました。
インターネットの情報ではパッケージインストールに Homebrew を使って解説している情報も多く、Ubuntu だとその都度 apt コマンドで置き換えてインストールするなど少し手間でした。今回 Homebrew が使えるようになったことで **Mac OS と同じ感覚で操作できる、コマンドの管理、パッケージの管理がしやすい**などの大きなメリットがあると感じました。

なにより sudo を使わずに手軽にパッケージマネージャーを使えるのが大きいです。Homebrew を思い切って導入したことで WSL2 での開発がさらに捗りそうです。

最後まで読んでいただきありがとうございました。

# 参考

- [Homebrew on Linux — Homebrew Documentation](https://docs.brew.sh/Homebrew-on-Linux)
- [Linux/WSLを正式サポート ～macOS向けパッケージマネージャー「Homebrew 2.0.0」が公開 - 窓の杜](https://forest.watch.impress.co.jp/docs/news/1167988.html)
- [Windows Terminal + WSL 2 + Homebrew + Zsh - Qiita](https://qiita.com/okayurisotto/items/36f6f9df499a74e62bff)
---
title: "Rust製ツールでおしゃれなターミナル環境を作る [Starship ✕ exa]"
emoji: "✨"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["starship","exa","shell","rust"]
published: false
---

# はじめに

エンジニアとして仕事をする中で PC からターミナルを使って作業することがとても多いです。エンジニアをしている人はこだわりが強く、それぞれ自分の作業しやすい環境を確立させている人が多い印象です。私も毎日使うものや作業環境は日々使いやすいようにカスタマイズするのが好きです。

エンジニアに限らず、日々使うものをこだわったり、自分の使いやすいようにカスタマイズすることでやる気が高まり作業効率のアップ、生産性の向上に繋がります。

ターミナル環境を構築する際に、GitHub などで Rust 製のコマンドやシェル環境が注目されているのが気になりました。

そこで「ターミナルをもっとおしゃれに使いたい」そんな思いから、今回は、`Starship` という Rust 製のプロンプトと Rust 製コマンド `exa` を活用してモダンかつ軽量、おしゃれなシェル環境を構築してみました。

作成したターミナルはこちらです。

![starship x exa で作成したターミナル画像](https://storage.googleapis.com/zenn-user-upload/ci6i4zza6ar0wna6l7zwfjszmsk3)

Starship の設定から作業ブランチ名と Node.js のバージョンを表示し、exa を使ってアイコン付きのツリー表示で React のプロジェクトを開いた様子です。Powerline などでおなじみのアイコンフォントの表示できるのでターミナルがいっきにおしゃれになります。

またターミナルのテーマには `Dracula` を使用しています。今回使用する Starship と色合いが似ており、テーマと合わせることで全体的に雰囲気がでておしゃれになりました。Dracula は Windows Terminal や iTerm2 など多くのツールに対応しています。

https://draculatheme.com/


# 今回使用する環境

今回は以下の環境にてそれぞれターミナルの環境構築をしました。両方の環境で同じ環境を作りました。

[Windows]
- Windows Terminal バージョン: 1.7.1033.0
- WSL2(Ubuntu 20.04 LTS)
- zsh 1.56.0

[Mac]
- iTerm2 Build 3.4.8
- macOS BigSur バージョン 11.3
- zsh 1.56.0

---

私は、開発で Windows と MacOS を両方使用しておりターミナルツールとしてそれぞれ Windows では[Windows Terminal](https://docs.microsoft.com/ja-jp/windows/terminal/)、Mac では[iTerm2](https://iterm2.com/)を使用しています。今回作成したターミナル環境は Rust 製ツールを活用しています。Rust 製ツールは互換性が重視されており異なる環境であっても簡単に同じような環境を作成できるのが大きなメリットです。

# Starshipを導入する

まず、Rust 製のシェルプロンプト「Starship」を導入します。

公式サイトはこちらです。
https://starship.rs/ja-jp/

「互換性優先」、「Rust 製」、「カスタマイズ可能」という三拍子揃ったモダンなシェルフレームワークになります。サイトを見てすぐわかりますが、公式が日本語のドキュメントを用意してくれています。デフォルトでカラフルな表示と絵文字の表示をしてくれるおしゃれなシェルです。互換性優先で設計されており、OS やシェルを選ばずにインストールできるため***複数の端末で同じ環境を揃えやすい、乗り換えやすいというメリット***があります。

## インストール

公式サイトにそれぞれの環境でのインストールが詳しく紹介されているので簡潔にまとめます。

以下のコマンドでバイナリをインストールします。
WSL2 で Ubuntu などを利用している場合はこちらでのインストールが簡単です。

```shell
$ sh -c "$(curl -fsSL https://starship.rs/install.sh)"
```

✓ Starship installed となれば完了です。また上の方法でインストールした場合、Starship の更新も同じコマンドで行います。アップデートする際に設定ファイルを変えることなくバージョンを置き換えてくれます。

また Mac などでパッケージ管理に Homebrew を使っている場合は以下のコマンドでもインストールできます。

```shell
$ brew install starship
```

## 初期設定 : Starshipの有効化

Starship を有効にするたの処理をシェルプロファイルへ記述する必要があります。

Zsh を使用している場合は以下のようにします。

```shell:zsh
# .zshrcの末尾に追記
echo eval "$(starship init zsh)" >> ~/.zshrc

# シェルの再読み込み
source ~/.zshrc
```

Bash を使用している場合は以下のようになります。

```shell:bash
# .bashrcの末尾に追記
echo eval "$(starship init bash)" >> ~/.bashrc

# シェルの再読み込み
source ~/.bashrc
```

シェルの再読み込みをすることで Starship が有効になります。

```shell
# デフォルトの表示
~
❯
```

:::message
`eval "$(starship init zsh)"`は必ずファイルの末尾に記述してある必要があります。シェルを再起動しても有効にならない場合は記述位置を見直して見てください。
:::

## 初期設定 : フォントの導入

Starship はアイコンフォントに対応しているため使用しているフォントがアイコンフォントに対応していないと一部文字化けする可能性があります。公式は NerdFont の導入を推奨しているため、Nerd フォントが手元にない場合は以下から探してみてください。また後述する `exa` というコマンドもアイコンフォントに対応しているため、導入することをおすすめします。

https://www.nerdfonts.com/

私は、[たわら様](https://twitter.com/tawara_san)が開発しているプログラミングフォント「白源 (はくげん／HackGen)」をターミナルのフォントに使用しています。

https://github.com/yuru7/HackGen

NerdFont を追加した「HackGenNerd Console」というフォントを公開しているためこちらをインストールして使用することでアイコンフォントも問題なく表示できます。またプログラミングフォントのため文字の識別がしやすい点もおすすめです。
## Starshipのカスタマイズ

Starship はデフォルトでもアイコンフォントの表示やカラフルな表示に対応しており便利なのですが、設定ファイルを作成して使用することでより細かい設定が可能となります。

```shell
$ starship config
```

とすることで `~/.config`の下に `starship.toml` という設定ファイルが作成され開かれます。

設定できる項目がかなり多く、自由なカスタマイズが可能です。詳しくは[設定 | Starship](https://starship.rs/ja-jp/config/)を見てみてください。上記の画像では以下の設定をしています。

```toml:starship.toml
# 空行追加
add_newline = true

# タイムアウト時間
scan_timeout = 10

# 記号の設定
[character]
success_symbol = "[▶](bold green)" # コマンド成功時
error_symbol = "[▶](bold red)"    # コマンド失敗時
```

# exaの導入

exa とは Linux でおなじみの `ls` コマンドを更に拡張したコマンドになります。こちらも Rust 製でとても扱いやすいものになっております。デフォルトでカラフルな表示とオプションを使用した詳細表示に対応しており前述した Starship ととても相性がいいです。

GitHub リポジトリはこちらになります。

https://github.com/ogham/exa

exa コマンドはインストール方法が多数ありディストリビューションによって大きく異なります。この記事内では Homebrew を使用したインストールと Cargo を使用したインストールを紹介します。

## Homebrew を使用したインストール

パッケージ管理に Homebrew を使用している場合は以下のコマンドでインストールできます。

```shell
$ brew install exa
```

WSL2 で Ubuntu を使用している場合であっても Homebrew を導入することで同じようにインストールできます。私は WSL2 において Homebrew を導入した上で exa コマンドを導入しています。WSL2 上の Ubuntu20.04 LTS に Homebrew を導入するやり方は過去に[こちらの記事](https://zenn.dev/ryuu/articles/wsl2-homebrew)にて紹介しています。

## Cargoを使用したインストール

Rust のビルドシステム兼パッケージマネージャーの `Cargo` を使用してインストールできます。

```shell
# Rustupをインストール
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

とすることで Rust のインストールが行われます。対話形式でインストールを進めていき `Rust is installed now. Great!` と表示されればインストールは成功です。

インストール直後にパスを通す必要があるので以下のコマンドでパスを通します。

```shell
source $HOME/.cargo/env
```

パスが通るとパッケージ管理ツールの `Cargo` が使用できるようになり、Cargo 経由で exa を導入できます。

```shell
$ cargo install exa
```

# exaを使ってみる

exa は ls コマンドと同じような感覚で使用できます。ターミナルで以下のように入力するだけでファイル名をカラフルに表示してくれます。

```shell
$ exa
```

ls コマンドの代わりに `exa` を使用することで全体的に文字をカラフルに表示してくれるためターミナルが一気に華やかになります。

また exa はオプションがかなり多く、オプションを組み合わせて柔軟な表示ができのも魅力です。
exa に続けてオプションを指定するのですが、オプションが覚えきれないくらい多いので詳しくは公式の ReadMe を参考にしてみてください。

また私は ls コマンドに慣れていることもあり、同じように使用したいので私は exa コマンドに対してエイリアスを設定しています。

# エイリアス設定でさらにexaを活用する

ls コマンドと同じように使えると聞いてピンときた人も多いのではないでしょうか。exa はエイリアスと組み合わせることで本領を発揮します。
前述したとおり exa はオプションがとても多く覚えて毎回入力するのは大変なので、ls コマンドに対してエイリアスを設定することで ls コマンドと同じような感覚で使用できるようになります。

私は ls コマンドと同じような使い方を保ちながら exa の恩恵を受けるために以下のようなエイリアスを設定しています。

```shell:.zshrc
if [[ $(command -v exa) ]]; then
  alias e='exa --icons --git'
  alias l=e
  alias ls=e
  alias ea='exa -a --icons --git'
  alias la=ea
  alias ee='exa -aahl --icons --git'
  alias ll=ee
  alias et='exa -T -L 3 -a -I "node_modules|.git|.cache" --icons'
  alias lt=et
  alias eta='exa -T -a -I "node_modules|.git|.cache" --color=always --icons | less -r'
  alias lta=eta
  alias l='clear && ls'
fi
```

内容としては exa コマンドがインストールされている環境でのみ有効になるエイリアスとしています。

私は常にリスト表示にアイコンを表示したいため、`e`、`l`、`ls`に対してベースにアイコンを表示するオプション `--icons` を付けてエイリアスにしています。`--git`とすることで Git のステータスも反映してくれるようになります。

またコマンドに `t` を混ぜることでツリー表示できるようにしています。
exa でのファイルツリー表示でもアイコンを合わせて表示するためにオプションを組み合わせています。ここまでくるとオプションがかなり長くなってしまいます。

そこでエイリアスを活用することで ls コマンドの上位互換としての運用が簡単にできるようになりました。

exa はオプションを組みわせることで柔軟なファイル表示ができるので各々使いやすいようにエイリアスを作成してみてください。

# おわりに
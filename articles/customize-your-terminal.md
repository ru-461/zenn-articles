---
title: "Rust製ツールでおしゃれなターミナル環境を作る【Starship ✕ exa】"
emoji: "✨"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["starship", "exa", "shell", "rust", "ターミナル"]
published: true
---

## はじめに

エンジニアとして働く中で、ターミナルを頻繁に利用することが一般的です。エンジニアは細部にこだわり、個々が自分にとって使いやすい作業環境を構築している印象があります。私も日々の作業や環境を使いやすくするため、様々なツールに関する情報を積極的に収集しています。

エンジニアに限らず、日々使うものをこだわる、自分の使いやすいようにカスタマイズすることでやる気が高まり**作業効率のアップ、生産性の向上に繋がります**。

Rust製のコマンドやツールなどが昨今注目されており、GitHubなどでおしゃれなOSSのコマンドやツールが増えてきました。そこで今回は、[Starship](https://github.com/starship/starship)というRust製のプロンプトとRust製コマンド[eza](https://github.com/eza-community/eza)を活用してモダンかつ軽量、おしゃれなターミナル、シェル環境を構築してみました。

今回作成したものがこちらです。

![starship x exa で作成したターミナル画像](/images/customize-your-terminal/image01.png)
*プロンプトはStarship、アイコンツリー表示にexaを使用*

Starshipで作業ブランチ名とNode.jsのバージョンを表示し、exaを使ってアイコン付きのツリー表示でReactのプロジェクトを開いた様子です。カラフルな表示とアイコンフォントで視認性を上げつつ、全体的にモダンな雰囲気になりました。

またターミナルのテーマにはDraculaを使用しています。今回使用するStarshipと色合いが似ており、テーマと合わせることで全体的に雰囲気がおしゃれになります。DraculaはWindows TerminalやiTerm2など多くのターミナルツールに対応しています。
https://draculatheme.com/

## 今回使用する環境

今回は以下の環境、バージョンにてそれぞれターミナルを構築しました。

---

[Windows]
- Windows Terminal 1.18.3181.0
- WSL2（Ubuntu 20.04 LTS）
- zsh 5.9

[Mac]
- iTerm2 Build 3.4.22
- macOS Sonoma 14.1.1
- zsh 5.9

---

私は、開発でWindowsとmacOSを両方使用しておりターミナルツールとしてそれぞれWindowsでは[Windows Terminal](https://docs.microsoft.com/ja-jp/windows/terminal)、Macでは[iTerm2](https://iterm2.com)を使用しています。Rust製ツールは互換性が重視されており、異なる環境であっても簡単に同じような環境を作成できるのが大きなメリットです。

## Starshipを導入する

まず、Rust製のシェルプロンプト「Starship」を導入します。
https://starship.rs/ja-jp/

GitHubのリポジトリは以下になります。
https://github.com/starship/starship

「互換性優先」「Rust製」「カスタマイズ可能」という三拍子揃ったモダンなプロンプトになります。サイトを見てすぐわかりますが、公式が日本語のドキュメントを用意してくれています。デフォルトでカラフルな表示と絵文字の表示をしてくれるおしゃれなプロンプトです。互換性優先で設計されており、OSやシェルを選ばずにインストールできるため**複数の端末で同じ環境を揃えやすい、乗り換えやすいというメリット**があります。

### インストール

公式サイトにそれぞれの環境でのインストールが詳しく紹介されているので簡潔にまとめます。

WSL2でUbuntuなどを利用している場合は以下のスクリプトを使ったインストールが簡単です。

```shell
$ curl -sS https://starship.rs/install.sh | sh
```

`✓ Starship installed `と表示されればインストールは完了です。またこのやり方でインストールした場合、Starshipの更新も同じコマンドで行います。アップデートする際は設定ファイルを変えることなく環境を引き続きながらバージョンを置き換えてくれます。

またMacなどでパッケージ管理に[Homebrew](https://brew.sh/ja/)を使っている場合は以下のコマンドでもインストールできます。

```shell
$ brew install starship
```

### 初期設定：Starshipの有効化

Starshipを有効にするたの処理をシェルプロファイルへ記述する必要があります。
現在使用しているシェルを確認するには以下のコマンドで確認可能です。

```shell
$ echo $SHELL

# Zshを使用している場合
/bin/zsh
```

Zshを使用している場合は以下のようにします。

```shell:zsh
# シェルプロファイル（.zshrc）の末尾に追記
echo eval "$(starship init zsh)" >> ~/.zshrc

# シェルプロファイルの再読み込み
source ~/.zshrc
```

Bashを使用している場合は以下のようになります。

```shell:bash
# シェルプロファイル（.bashrc）の末尾に追記
echo eval "$(starship init bash)" >> ~/.bashrc

# シェルプロファイルの再読み込み
source ~/.bashrc
```

シェルの再読み込みをすることでStarshipが有効になりプロンプトの表示が変化します。

```shell
# StarShip導入後のプロンプト表示
❯
```

:::message
`eval "$(starship init zsh)"`・`eval "$(starship init bash)"`は必ず**シェルプロファイルの末尾**に記述してある必要があります。シェルを再起動しても有効にならない場合は記述位置を見直してみてください。
:::

### 初期設定：フォントの導入

Starshipはアイコンフォントに対応しているため使用しているフォントがアイコンフォントに対応していないと一部文字化けする可能性があります。また後述するezaもアイコンフォントに対応しているため、NerdFont、NerdFontを含むフォントを導入することをおすすめします。

私は、[たわら様](https://twitter.com/tawara_san)が開発しているプログラミングフォント「白源（はくげん ／ HackGen）」をターミナルのフォントに使用しています。
https://github.com/yuru7/HackGen

NerdFontを追加した「HackGenNerd Console」というフォントがあるため、こちらをインストールして使用することでアイコンフォントも問題なく表示できます。またプログラミングフォントのため全角スペース・半角スペースなどの識別がしやすい点もおすすめです。

### Starshipのカスタマイズ

Starshipはデフォルトでもアイコンフォントの表示やカラフルな表示に対応しており便利なのですが、設定ファイルを作成して使用することでより細かい設定が可能となります。

下記のコマンドでStarshipの設定ファイルを作成します。

```shell
$ starship config
```

設定ファイルの保存先はデフォルトで~/.config/starship.tomlとなります。

Starship設定できる項目がかなり多く、自由なカスタマイズが可能です。詳しくは[設定 | Starship](https://starship.rs/ja-jp/config)を見てみてください。

以下に記事内の画像にて使用している設定を記載します。

```toml:starship.toml
# 空行追加
add_newline = true

# タイムアウト時間
scan_timeout = 10

# シンボル設定
[character]
success_symbol = "[▶](bold green)"  # コマンド成功時
error_symbol   = "[▶](bold red)"    # コマンド失敗時
```

## ezaを導入する

ezaとはLinuxでおなじみの`ls`を拡張したコマンドになります。こちらもRust製でとても扱いやすいものになっております。デフォルトでカラフルな表示とオプションを使用した詳細表示に対応しており前述したStarshipととても相性がいいです。
https://eza.rocks

GitHubリポジトリは以下になります。
https://github.com/eza-community/eza

ezaコマンドはインストール方法が多数ありディストリビューションによって大きく異なります。この記事内ではパッケージマネージャHomebrewを使用したインストールとRustのパッケージマネージャであるCargoを使用したインストールの2つを紹介します。

### Homebrewを使用したインストール

パッケージ管理にHomebrewを使用している場合は以下のコマンドでインストールできます。

```shell
$ brew install eza
```

WSL2でUbuntuを使用している場合であってもHomebrewを導入することで同じようにインストールできます。私はWSL2においてHomebrewを導入した上でコマンドを導入しています。WSL2上のUbuntu20.04 LTSにHomebrewを導入するやり方は過去に[こちらの記事](https://zenn.dev/ryuu/articles/wsl2-homebrew)にて紹介していますので参考にしてください。

### Cargoを使用したインストール

Rustのビルドシステム兼パッケージマネージャーのCargoを使用してexaを導入する方法です。
Cargoを使用するためにRustの環境を作る必要があるのですが、今回はrustupというツールを使い簡単に始めます。
https://www.rust-lang.org/ja/tools/install

rustupとはRustの公式ドキュメント内でも推奨されているツールで、簡単にRustのインストールと開発環境のセットアップを行ってくれる便利なツールです。

```shell
# rutupをインストール
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

インストール中にいくつかの質問が表示されますが、基本的にはそのままで問題ありません。
カスタマイズする場合は選択を変更して適宜設定してください。

ターミナル上に`Rust is installed now. Great!`と表示されればインストールは成功です。
次回ターミナル起動時に自動的にパスが通ります。すぐに使用したい場合は、以下のようにすると自動的にパスを通してくれます。

```shell
# rustupが用意してくれているスクリプトを実行してパスを通す
source $HOME/.cargo/env
```

インストールが終わったらバージョンを確認してみます。
以下のコマンドでバージョンを確認し、アップデートがあれば最新の状態にアップデートしておきます。

```shell
# rustupのバージョンを確認
$ rustup --version

# rustupをアップデート
$ rustup self update
```

ここまでできたらCargoが使用できるため、以下のコマンドでezaをインストールできます。

```shell
$ cargo install eza
```

:::message
Ubuntuなどの環境によっては`error: linker cc not found`と表示されビルドに失敗する場合があります。これはgccとその依存関係がないことによるエラーのため事前に`apt install build-essential`で必要なコマンドをインストールした上で再試行してみてください。
:::

# ezaを使ってみる

ezaはlsコマンドの代替コマンドのためlsと同じ感覚で使用できます。ターミナルで以下のように入力するだけでファイル名をカラフルに表示してくれます。

```shell
# ezaを使用してファイルをリスト表示
$ eza
```

lsコマンドの代わりにezaを使用することで全体的に文字をカラフルに表示してくれるためターミナルが一気に華やかになります。

またezaはオプションがかなり多く、オプションを組み合わせて柔軟な表示ができのも魅力です。
ezaに続けてオプションを指定するのですが、オプションが覚えきれないくらい多いので詳しくは[Command-line options](https://github.com/eza-community/eza?tab=readme-ov-file#command-line-options)を参考にしてみてください。

## エイリアス設定でさらにexaを活用する

lsコマンドと同じように使えると聞いてピンときた人も多いのではないでしょうか。exaはエイリアスと組み合わせることで本領を発揮します。
前述したとおりexaはオプションがとても多く覚えて毎回入力するのは大変なので、lsコマンドに対してエイリアスを設定することでlsコマンドと同じような感覚で使用できるようになります。

私はlsコマンドと同じような使い方を保ちながらexaの恩恵を受けるために以下のようなエイリアスを設定しています。

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

内容としてはexaコマンドがインストールされている環境でのみ有効になるエイリアスとしています。

lsをベースに、ファイルの種類ごとにアイコンを表示するオプション`--icons`を付けてエイリアスにしています。また`--git`オプションを付与することでGitのファイル管理ステータスもリストに反映してくれるようになります。

またコマンドに`t`を混ぜることでツリー表示できるようにしています。
exaでのファイルツリー表示でもアイコンを合わせて表示するためにオプションを組み合わせています。ここまでくるとオプションがかなり長くなってしまいます。
オプションが長くなる問題の解決策としてエイリアスを活用し`ls`の上位互換としての運用がしやすいようにカスタマイズしております。

エイリアスの設定例としてはこちらのサイトを参考にカスタマイズさせていだたきました。わかりやすい解説ありがとうございます。
https://tombomemo.com/exa-install-settings/

exaはオプションを組みわせることで柔軟なファイル表示ができるので、各々使いやすいようにエイリアスを作成してみてください。

## おわりに

今回は、**おしゃれなターミナル**というテーマでターミナルとシェルのカスタマイズをしてみました。ターミナル環境をカスタマイズすることで気分転換にもなりターミナルを開くたびにモチベーションが高まるように感じます。また作業環境にこだわるという点で生産性が上がるのを体感することが出来ました。

Rust製のツールを初めて使いましたが、互換性が高かったり、安全かつ高速だったりとメリットが多く更に調べて取り入れていきたいと感じました。この記事をきっかけに、モダンな技術で柔軟にカスタマイズできるターミナルを簡単に構築できることに少しでも魅力を感じてもらえたら幸いです。

最後まで読んでいただきありがとうございました。

## 参考

- [とってもかわいいプロンプトstarshipで2021年宇宙の旅に出よう](https://zenn.dev/aoi_avant/articles/9ac01d7857add9)
- [The Rust Programming Language 日本語版 - The Rust Programming Language 日本語版](https://doc.rust-jp.rs/book-ja/title-page.html)
- [lsよりもexaじゃん! Rust製Linuxコマンド 【exa】 | TomboMemo](https://tombomemo.com/exa-install-settings)
- ["error: linker `cc` not found" on Ubuntu 18.04 LTS, using X11 · Issue #1440 · alacritty/alacritty](https://github.com/alacritty/alacritty/issues/1440)

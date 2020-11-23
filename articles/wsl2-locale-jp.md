---
title: "WSL2を日本語化するときにやったこと"
emoji: "♨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["wsl", "Windows", "初心者", "ubuntu"]
published: false
---

私は、メインの開発環境として Windows10 の WSL2 を使っています。初期状態では日本語ロケールが含まれておらず、表示はすべて英語です。この記事では日本語ロケールを追加して日本語表示に対応させます。それと同時に時刻の日本時間化、マニュアル日本語化を行います。 WSL 上で開発環境を作成するときに毎回行う作業で忘れがちなので備忘録として残しておきます。

# 環境

- Windows10 20H2
- Windows Subsystem for Linux 2(WSL2)
- Ubuntu 20.04.1 LTS (Focal Fossa)

# Microsoft Store から WSL2 へ Ubuntu をインストール

![](https://storage.googleapis.com/zenn-user-upload/m9gsq8lu1u49qs989num6iyl7r8e)
_インストールができたら「起動」をクリック_
インストールが終わったら「起動」をクリックでシェルが起動します。

# 初期設定

### ユーザーアカウントとパスワードを作成

ユーザアカウントとパスワードを作成するように促されるので、指示のとおりにアカウントを作成します。
:::message
パスワードは 2 回確認されるので同じものを入力。
:::

### デフォルトの WSL のバージョンを設定

```bash
$ wsl --set-default-version 2
```

## アップデート

```bash
$ sudo apt update && sudo apt upgrade
```

コマンドがインストールできないのを防ぐために、使用可能パッケージのリスト更新とアップグレードを先に行います。

## 使用可能なロケールを確認する

```bash
$ locale -a
```

この時点では日本語ロケールが含まれていないためインストールしていきます。

## 日本語の言語パックをインストール

```bash
$ sudo apt install -y language-pack-ja
```

パスワードが求められたら root 権限のパスワード(初回に設定したパスワード)を入力して続行します。

`locale -a`コマンドで確認。

```bash
C
C.UTF-8
POSIX
en_US.utf8
ja_JP.utf8
```

一番下に日本語ロケール(ja_JP.utf8)が追加されました。

## 日本語ロケールを設定

```bash
$ sudo update-locale LANG=ja_JP.UTF8
```

インストールが終わったら `exit` でシェルを終了し、再起動します。

:::message

以下のコマンドでデフォルトロケール(英語)に戻すこともできます。

```bash
$ sudo update-locale LANG=en_US.UTF8
```

:::
これで日本語ロケールの導入ができました。しかし、デフォルトではタイムゾーンが日本時間になっていません。せっかくなのでタイムゾーンも日本時間(JST)に設定していきます。

## タイムゾーンの変更

```bash
$ sudo dpkg-reconfigure tzdata
```

GUI 表示になるので選択肢から「アジア」→「東京」を選択します。
![](https://storage.googleapis.com/zenn-user-upload/hms81758oplrwgqztsc3d5lt2oql)
_アジアを選択_
![](https://storage.googleapis.com/zenn-user-upload/wlmalkrups5z43uhweg3wnykiwkv)
_東京を選択_
`date`コマンドで現在の時刻を表示。

```bash
2020年 11月 23日 月曜日 21:48:00 JST
```

日本時間(JST)で表示されています。
これで WSL2 で日本時間が使用できるようになりました。

## コマンドのマニュアル表示も日本語にする

```bash
$ sudo apt -y install manpages-ja manpages-ja-dev
```

実行することでマニュアルコマンド($man)の表示が日本語で表示されるようになります。

```basg
$ man apt

名前
       apt - コマンドラインインターフェイス

概要
       apt [-h] [-o=設定文字列] [-c=設定ファイル] [-t=対象リリース] [-a=アーキテクチャ] {list | search | show |
           update | install パッケージ [{=パッケージバージョン番号 | /対象リリース}]...  | remove パッケージ...  |
           upgrade | full-upgrade | edit-sources | {-v | --version} | {-h | --help}}

説明
       apt は、パッケージ管理システム用の高レベルのコマンドラインインターフェースを提供します。エンドユーザインター
       フェースとして設計されています。また apt-get(8) や apt-cache(8) のような専用の APT ツールと比べて、デフォルト
       でインタラクティブな使用に適したいくつかのオプションが有効になっています。

       apt 自身と同じように、man ページはエンドユーザインターフェースとして意図されています。さらに、一部のオプション
       や詳細の豊富さで読者を圧倒することを避けるため、複数の場所で部分的に情報を複製しないよう、最も使用されるコマン
       ドとオプションを言及するように意図されています。

       update (apt-get(8))
           update は、設定されたすべての取得元からパッケージ情報をダウンロードするために使用されます。ほかのコマンド
           は、このデータを操作します。例えば、パッケージのアップグレードを実行したり、中を検索したり、インストール可
           能なすべてのパッケージに関する詳細情報を表示します。

       upgrade (apt-get(8))
           upgrade は、sources.list(5) で設定された取得元からシステムに現在インストール済みのすべてのパッケージで利用
           可能なアップグレードをインストールするために使用されます。依存関係を満たすために必要な場合は新しいパッケー
           ジがインストールされますが、既存のパッケージが削除されることはありません。パッケージのアップグレードにイン
           ストール済みパッケージの削除が必要な場合、そのパッケージのアップグレードは行われません。
```

apt コマンドのマニュアルを表示したところしっかりと日本語表示されています。

# おわりに

WSL2 で日本語表示ができる環境を作ることができました。コマンドのタイポをしたときなどに日本語でヒントが表示されるので、すぐに気づくことができたりとメリットがあると感じました。誰かの参考になれば幸いです。

### 参考

- https://www.atmarkit.co.jp/ait/articles/1806/28/news043.html
- https://teratail.com/questions/262291

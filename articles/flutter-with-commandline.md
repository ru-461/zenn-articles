---
title: "Windowsのコマンドラインを駆使してFlutterの環境構築をしてみた"
emoji: "🛫"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["flutter","windows","androidstudio","scoop","winget"]
published: false
---

# はじめに

Mac で開発環境を構築する際に Homebrew などのパッケージ管理マネージャーを使って構築している記事をよく見かけます。パッケージマネージャーを使うことで、コマンドラインでパッケージの依存関係も解決してインストールできるため、開発環境構築に重宝されます。そのメリットの 1 つに、コマンドのインストールと同時にパスも通してくれるためすぐにコマンドの実行ができ、コマンドの場所を IDE(統合開発環境)側に教えてあげることですぐに開発に取りかかれる点があります。

Windows の場合はプログラミング言語や IDE もインストーラーを用いたインストールが多く、パスを通すために各自で直接環境変数を触るなど少し手間になります。例えば、Java の開発を始める際に JDK の環境変数をいじって導入している記事をよく見かけます。Java の開発を始める前の環境構築の難易度が少し高く、環境変数、パス周りの話がわからないと少しハードルが高いため億劫うです。過去の私がそうでした。Windows の環境変数を編集するときにシステム側の変数を触ってしまって PC の起動すらできなくなり、泣く泣く PC を初期化した記憶があります。このことから、なるべくであれば環境変数を触らず、できるのであれば環境を汚すことなく作業したいというのが本音です。パッケージマネージャーの仕組みに感動した記憶があります。

## Windowsにおけるパッケージマネージャー

Mac には `homebrew` という強力なパッケージマネージャーが存在しますが、Windows にも 10 年ぐらい前からパッケージ管理の仕組みをもつソフトがいくつか存在し、今現在も開発が続けられています。そして今、Microsoft が [Windows Package Manager Client](https://github.com/microsoft/winget-cli) として公式のパッケージマネージャーを開発しており。**2021 年 4 月現在プレビュー版**として提供されております。コマンドライン(コマンドプロンプト・PowerShell)からパッケージのインストールを行えるようにできるため、Unix 系 OS のような環境構築ができるような日も近いです。

今回は、Windows のパッケージマネージャである、[Scoop](https://scoop.sh/)と Windows Package Manager Client を使用して Flutter のアプリを Android Studio からビルドして実行できるところまでやってみます。
## Flutter とは

Google が開発している UI ツールキットになります。Dart 言語で書かれており、単一のコードからモバイル・Web・デスクトップなどの環境を問わずに実行できるアプリを作成できることで有名です。最近はモバイルアプリ開発を始める人が Flutter から学び始めている例を多く見るため、どんどん盛り上がってる印象を受けます。Flutter は SDK のインストールとプラグインのインストールを行うことで AndroidStudio や Xcode といった IDE を使用して開発することが出来ます。この記事では Windows を対象としているため、Windows 版 AndroidStudio を使用します・

# インストール検証環境

OS は Windows10 を想定しています。

| ツール | バージョン |
| ---- | ---- |
| Android Studio | 4.1.2.0 |
| Flutter | Text |
| Windows Package Manager | v0.2.10771 プレビュー |
| Scoop | 3d67b7d3 |

# Windows Package Manager の準備

Windows Package Manager を使用してコマンドで Android Studio をインストールしていきます。
Windows Package Manager は Microsoft Store からアプリインストーラーを更新することで使用できるようになります。

Windows のスタートメニューから Microsoft Store を起動して「アプリ インストーラー」と検索するか、以下のリンクから飛べます。

https://www.microsoft.com/ja-jp/p/%E3%82%A2%E3%83%97%E3%83%AA-%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%A9%E3%83%BC/9nblggh4nns1?activetab=pivot:overviewtab

インストールページが開かれます。

![アプリインストーラーの詳細情報の画像](https://storage.googleapis.com/zenn-user-upload/fn77b126s2ljhedbmolu1yerp56z)
**すでに更新されている場合はこの様になります**

インストールが終わったら PowerShell を**管理者権限で起動**します。
ターミナルに `winget` と入力するとコマンドの使用例とヘルプが表示されます。winget とは Windows Package Manager 用のコマンドです。

```powershell:powershell
> winget

Windows Package Manager v0.2.10771 プレビュー
Copyright (c) Microsoft Corporation. All rights reserved.

WinGet コマンド ライン ユーティリティを使用すると、コマンド ラインからアプリケーションやその他のパッケージをインストールできます。

etc ...
```

ここまで表示できたら Windows Package Manager のインストールが完了です。

最初に以下のコマンドでソースを最新に更新しておきます。

```powershell:powershell
> winget source update

すべてのソースを更新しています...
ソースを更新しています: winget...
完了
```
Winget コマンドでインストールできるパッケージ一覧を出力してみます。
```powershell:powershell
> winget search
```
たくさんのアプリがずらっと表示されます。この一覧にあるアプリがインストール可能です。
この中から Android Studio を探してみます。`searchコマンド`に続けてアプリ名を入力します。

```powershell:powershell
> winget search AndroidStudio

名前           ID                   バージョン
----------------------------------------------
Android Studio Google.AndroidStudio 4.1.2.0
```
Android Studio が見つかりました。それではインストールしていきます。
```powershell:powershell
> winget install Google.AndroidStudio

見つかりました Android Studio [Google.AndroidStudio]
このアプリケーションは所有者からライセンス供与されます。
Microsoft はサードパーティのパッケージに対して責任を負わず、ライセンスも付与しません。
Downloading https://redirector.gvt1.com/edgedl/android/studio/install/4.1.2.0/android-studio-ide-201.7042882-windows.exe
  ██████████████████████████████   896 MB /  896 MB
インストーラーハッシュが正常に検証されました
パッケージのインストールを開始しています...
インストールが完了しました
```

`インストールが完了しました`となればインストール成功です。うまくいかない場合は管理者権限にてコマンドを実行しているかを確認してください。

Windows のスタートメニューを確認すると Android Studio がインストールされているのが確認できます。

![スタートメニューからAndroid Studioを確認した様子](https://storage.googleapis.com/zenn-user-upload/ap161h386hbtb8xs86ggwyo5q14u)

公式サイトからインストーラーをダウンロードしてローカルで実行、GUI をポチポチとすることなく Android Stuido をインストールできました。インストールにかかる時間もそれほどかからないです。
# Flutterの導入

ここまでコマンドだけで Android Studio をインストールできました。続いて `Scoop` というパッケージマネージャーを導入して Flutter と依存関係をまとめて導入します。
Scoop とは Windows 向けのパッケージマネージャーの 1 つでパッケージ管理に管理者権限を使わないという特徴を持ちます。

【公式サイト】
https://scoop.sh/

【GitHub】
https://github.com/lukesampson/scoop

## Scoopのインストール

インストールするために以下のスクリプトを PowerShell から実行します。

```powershell:Powershell
$ iwr -useb get.scoop.sh | iex
```
うまく実行できないときは以下のコマンドでユーザーポリシーを緩めることでインストールできます。
```powershell:Powershell
$ Set-ExecutionPolicy RemoteSigned -scope CurrentUser
```
Scoop コマンドが実行できることを確認できればインストールは完了です。
```powershell:Powershell
$ scoop

Usage: scoop <command> [<args>]
...
```

Scoop コマンドが使えることを確認できたら、続けて Flutter を導入します。
Scoop には、`Bucket`という概念が存在し、Bucket を追加することでインストールできるアプリを増やすことができます。デフォルトでは `main Bucket` しか追加されていません。
Flutter 関連のツールは `Java Bucket` に存在するため、予め追加しないとインストールできません。

以下のコマンドで Bucket を追加出来ます。

```powershell:powershell
 > scoop bucket add java
```
Bucket の追加が完了したら続けて Flutter の開発に必要なパッケージをまとめて導入していきます。

以下のコマンドを実行します。

```powershell:powershell
> scoop install flutter
```

Flutter のインストールが始まります。インストール途中で以下が依存関係として順番にインストールされます。

- adb [64bit]
- android-sdk [64bit]
- adopt8-hotspot  [64bit]
- flutter [64bit]

最後に Android SDK のライセンスが表示されるので、`y`を押して承諾していきます。

続けてインストールする SDK プラットフォームを選択します。デフォルトで一番最新のものが選択されます。こだわりがなければデフォルトで問題ありません。

```powershell:powershell
Available platforms:
[1] platforms;android-7
[2] platforms;android-8
[3] platforms;android-9
[4] platforms;android-10
[5] platforms;android-11
[6] platforms;android-12
[7] platforms;android-13
[8] platforms;android-14
[9] platforms;android-15
[10] platforms;android-16
[11] platforms;android-17
[12] platforms;android-18
[13] platforms;android-19
[14] platforms;android-20
[15] platforms;android-21
[16] platforms;android-22
[17] platforms;android-23
[18] platforms;android-24
[19] platforms;android-25
[20] platforms;android-26
[21] platforms;android-27
[22] platforms;android-28
[23] platforms;android-29
[24] platforms;android-30
For a list of platforms and what they mean, see: https://developer.android.com/about/dashboards/
No platform detected. Please select a platform to install [Default: 24]:
```

再度ライセンスに承諾することで SDK Platform のインストールが行われます。

インストールが終了する際に `flutter doctorコマンド` が走り、状態をチェックしてくれます。これで Flutter のインストールは完了です。

Android toolchain でエラーがでますので、Android Studio 側で修正していきます。
# おわりに
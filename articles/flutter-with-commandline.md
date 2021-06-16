---
title: "Windows のコマンドラインを駆使して Flutter の開発環境を構築してみた"
emoji: "🛫"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["flutter", "windows", "androidstudio", "scoop", "winget"]
published: true
---

# はじめに

Mac で開発環境を構築する際に Homebrew などのパッケージマネージャーを使って構築している記事をよく見かけます。パッケージマネージャーを使うことで、コマンドラインでパッケージの依存関係も解決してインストールできるため、開発環境構築の際に重宝されます。パッケージマネージャーを使うメリットに、コマンドのインストールと同時にパスも通してくれるためすぐにコマンドの実行ができること。コマンドのある場所を IDE(統合開発環境)などに教えてあげることですぐに開発に取りかかれるという点があります。

Windows の場合はプログラミング言語や IDE もインストーラーを用いたインストールが多く、パスを通すために各自で直接システム環境変数を触るなどの作業が発生し少し手間になります。例えば、Windows のコマンドプロンプトで Java のソースコードのコンパイルをしたくて、 JDK をダウンロード → bin ディレクトリの場所(パス)をシステム環境変数に追加しパスを通す作業をした経験があるのではないでしょうか。

開発を始める前の環境構築で頻出するシステム環境変数、パス周りの概念がわからないと初学者にとって環境構築は少しハードルが高いように感じます。私がプログラミングを勉強し始めたとき、Windows の環境変数編集から管理者権限で間違えて他のシステム側の変数を消してしまったため PC の起動ができなくなり、泣く泣く Windows を再インストールしたことがあります。その経験から、Windows 上でなるべくシステム環境変数を触らず、環境を汚すことなく手軽に開発環境を構築できればいいなと感じていました。そんなときに、Linux の `apt` や `yum` といったパッケージマネージャーと出会い、その仕組みや手軽さに感動した記憶があります。

## Windowsにおけるパッケージマネージャー

Mac には [Homebrew](https://brew.sh/index_ja) という強力なパッケージマネージャーが存在しますが、Windows にも 10 年ぐらい前からパッケージ管理の仕組みをもつソフトがいくつか存在し、今現在も開発が続けられています。そして現在、Microsoft が [Windows Package Manager Client](https://github.com/microsoft/winget-cli) として公式のパッケージマネージャーを開発しており。**2021 年 4 月現在でプレビュー版**として提供しております。パッケージマネージャーを使うことでコマンドライン(コマンドプロンプト・PowerShell)からパッケージのインストールや依存関係の解決ができるため、Windows でも Unix 系 OS のような柔軟な環境構築ができるような日も近いのではと感じます。

今回は、Windows のパッケージマネージャである、[Scoop](https://scoop.sh/)と Windows Package Manager Client を使用して Flutter のアプリを Android Studio からビルドして実行できるところまでやってみます。

[Flutter の公式のドキュメント](https://flutter.dev/docs/get-started/install/windows)でも紹介されていますが、Flutter の本体を公式サイトからダウンロードして、ローカルに配置、システム環境変数に Flutter のパスを追加する方法が一般的だと思われます。ですが、今回はなるべく環境変数を触らずにコマンドでパッケージマネージャーを操作して Flutter のアプリの編集と実行できる環境を構築することを目標に環境構築をしました。
## Flutter とは

https://flutter.dev/

Google が開発している UI ツールキットになります。Dart 言語で書かれており、単一のコードからモバイル・Web・デスクトップなどの環境を問わずに実行できるアプリを作成できる「クロスプラットフォーム開発」が特徴です。最近はモバイルアプリ開発を始める人が Flutter から学び始めている例を多く見るため、どんどん盛り上がってる印象を受けます。Flutter は SDK のインストールとプラグインのインストールを行うことで AndroidStudio や Xcode といった IDE を使用して開発することが出来ます。

# インストール検証環境

OS には `Windows10` を使用しています。

| ツール | バージョン |
| ---- | ---- |
| Windows Package Manager | v0.2.10771 プレビュー |
| Scoop | 3d67b7d3 |
| Android Studio | 4.1.2.0 |
| Flutter | 2.0.4 |

# Windows Package Manager の準備

Windows Package Manager を使用してコマンドで Android Studio をインストールしていきます。
Windows Package Manager(winget-cli) は Microsoft Store からアプリインストーラーを更新することで使用できるようになります。

Windows のスタートメニューから Microsoft Store を起動して「アプリ インストーラー」と検索するか、以下のリンクから飛べます。

https://www.microsoft.com/ja-jp/p/%E3%82%A2%E3%83%97%E3%83%AA-%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%A9%E3%83%BC/9nblggh4nns1?activetab=pivot:overviewtab

インストールページが開かれます。

![アプリインストーラーの詳細情報の画像](https://storage.googleapis.com/zenn-user-upload/fn77b126s2ljhedbmolu1yerp56z)
**すでに更新されている場合はこの様になります**

インストールが終わったら PowerShell を**管理者権限で起動**します。
ターミナルに `winget` と入力するとコマンドの使用例とヘルプが表示されます。winget とは Windows Package Manager を操作するためのコマンドです。

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
この中から Android Studio を探してみます。`searchコマンド` に続けてアプリ名を入力します。

```powershell:powershell
> winget search AndroidStudio

名前           ID                   バージョン
----------------------------------------------
Android Studio Google.AndroidStudio 4.1.2.0
```

Google が提供する、Android Studio が見つかりました。それではインストールしていきます。

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

`インストールが完了しました` となればインストール成功です。うまくいかない場合は管理者権限にてコマンドを実行しているかを確認してください。

Windows のスタートメニューを確認すると Android Studio がインストールているのが確認できます。

![スタートメニューからAndroid Studioを確認した様子](https://storage.googleapis.com/zenn-user-upload/ap161h386hbtb8xs86ggwyo5q14u)

公式サイトからインストーラーをダウンロードしてローカルで実行、GUI をポチポチとすることなく Android Stuido をインストールできました。インストールに時間もそれほどかからないです。
# Flutter の導入

ここまでコマンドだけで Android Studio をインストールできました。続いて `Scoop` というパッケージマネージャーを導入して Flutter と依存関係をまとめて導入します。
Scoop とは Windows 向けのパッケージマネージャーの 1 つでパッケージ管理に管理者権限を使わないという特徴を持ちます。

Scoop はコマンドツール以外にも Android Studio や VSCode ような GUI をもつアプリケーションも同じくインストールできます。しかし、Scoop でアプリケーションをインストールする場合、インストール先が Windows が想定するディレクトリ構成と異なるため不具合に繋がる可能性もあります、そのため私は現在 GUI ベースのアプリケーションを Windows Package Manager、CUI ベースのツールを Scoop と分けて管理しています。

【公式サイト】
https://scoop.sh/

【GitHub】
https://github.com/lukesampson/scoop

## Scoop のインストール

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

コマンドの使い方と使用可能なオプションが表示されます。

## Flutter を Scoop からインストール

Scoop コマンドが使えることを確認できたら、続けて Flutter を導入します。
Scoop には、`Bucket` という概念が存在し、Bucket を追加することでインストールできるアプリを増やすことができます。デフォルトでは `main Bucket` しか追加されていません。
Flutter 関連のツールは `extras Bucket` と `java Bucket` に存在するため、予め追加しないとインストールできません。

以下のコマンドで Flutter のインストールに必要な Bucket をまとめて追加出来ます。

```powershell:powershell
 > scoop bucket add java extras versions
```

Bucket の追加が完了したら続けて Flutter の開発に必要なものを導入していきます。

以下のコマンドを実行します。

```powershell:powershell
> scoop install flutter
```

Flutter のインストールが始まります。インストール途中で以下が依存関係として順番にインストールされます。

- adb[64bit]
- android-sdk[64bit]
- adopt8-hotspot[64bit]
- flutter[64bit]

依存関係としてなにがインストールされるかは `depends コマンド` で都度確認できます。

```powershell:powershell
> scoop depends  flutter

adopt8-hotspot
android-sdk
adb
```

最後に Android SDK のライセンスが表示されるので、`y` を押して承諾していきます。

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
この時点で Flutter にパスが通っているため、Windows の環境変数の編集からパスを通す必要はありません。便利ですね。

しかし、doctor コマンドを実行したときに Android toolchain でエラーがでますので、Android Studio 側で修正していきます。

# Android Studio の初期設定

最初に、Winget でインストールした Android Studio を起動します。最初の設定で Android SDK が見つからないとエラーになります。

![Android SDKのエラー画面](https://storage.googleapis.com/zenn-user-upload/svy0zfj5pariecx1pgxb1xxzdzpp)

ここで Android SDK の場所を設定してあげます。
Scoop でインストールしたものはユーザーデフォルトの下 `~\scoop\` の中にすべてまとめられています。

Scoop でインストールしたアプリとライブラリの格納場所は以下のコマンドで調べることが出来ます。

```powershell:powershell
# インストール先を絶対パスで取得
> scoop prefix android-sdk

C:\Users\user\scoop\apps\android-sdk\current
```

`Android SDK Location:` に上のコマンドで取得した SDK へのフルパスを指定し続行します。

![Android SDKのパスを指定する様子](https://storage.googleapis.com/zenn-user-upload/tydpgb49kcjlldov0enf5odlava9)

パスを指定するとその他必要なコンポーネントのダウンロードが始まるので終わるまで待ちます。
Android Studio のメインメニューが表示されればセットアップは完了です。

SDK の場所のセットアップが終わった後に、PowerShell にてもう一度 `flutter doctor` コマンドを実行します。

```powershell:powershell
> flutter doctor

Doctor summary (to see all details, run flutter doctor -v):
[!] Android toolchain - develop for Android devices (Android SDK version 30.0.3)
    ! Some Android licenses not accepted.  To resolve this, run: flutter doctor --android-licenses

```

上から 2 番目の `Android toolchain` の内容が変わっています。追加でライセンスに承諾する必要があるため、以下のコマンドでライセンスに承諾します。

```powershell:powershell
> flutter doctor --android-licenses
```

もう一度 `flutter doctor` を実行して `• No issues found!` と出力されれば Flutter の導入は完了です。

## Flutter 開発プラグインの導入

Flutter のプロジェクトを作成するためにプラグインが必要になるためインストールします。
Android Studio のメニュー画面から「Configure」→「Plugins」と進みます。

![Pluginを選択する様子](https://storage.googleapis.com/zenn-user-upload/67u844h1vme47mw0s99zn5izti78)

表示されるダイアログの中でタブを「Marketplace」に切り替えて「Flutter」と検索してインストールします。

![Flutterプラグインをインストールする様子](https://storage.googleapis.com/zenn-user-upload/lfa6bx1hlltz39mb5zwf1b6d1saa)

これで Flutter の開発プラグインの導入は完了です。合わせて **Dart のプラグインも必要になるのですが Flutter のプラグインをインストールするときに合わせてインストールされる**ので気にしなくて問題ありません。

Android Studio を再起動すると、「Create New Flutter Project」の項目が増えているので、ここから新規 Flutter プロジェクトを作成できるようになります。うまく反映されない場合はプラグイン画面から「Dart」と「Flutter」プラグインが `Enable` になっていることを再度確認してください。

![再起動したあとのメインメニュー](https://storage.googleapis.com/zenn-user-upload/fvqw55wf58miyulelw3v82ek1qik)
***Create New Flutter Projectが追加された***

画面の指示に従ってプロジェクト作成をしていくのですが、途中で Flutter SDK のパスを指定する箇所があります。

![SDKへのパスを指定する画面の画像](https://storage.googleapis.com/zenn-user-upload/0wlvhw8a7ds549qvv8sdxfy74ddx)
***デフォルトでSDKへのパスが指定されていないので入力***

インストールした Flutter SDK のパスは Android SDK と同じように以下のコマンドで取得できます。

```powershell:powershell
# インストール先を絶対パスで取得
> scoop prefix android-sdk

C:\Users\user\scoop\apps\flutter\current
```

`Flutter SDK path` のところに `C:\Users\ユーザー名\scoop\app\scoop\apps\flutter\current` を指定して続行します。

そのまま続行すると新規 Flutter プロジェクトが Android Studio 上で開かれます。

# Flutter アプリを実行する

Android Studio 上でプロジェクトが開かれるのを確認したら、実際に実行できるか試します。

上部のメニューから `Chrome(Dev)` を選択して `Shift + F10` で実行します。

![ChromeでFlutterが実行される様子](https://storage.googleapis.com/zenn-user-upload/ydtjzfg7l2yes4wn5retozzl2xyy)
***Flutter Demo Home Page***

自動的に Chrome 上で Flutter のデモアプリが起動しました。ホットリロードに対応しているためコードの変更がリアルタイムに反映されます。

AVD マネージャーから Android Emulator を作成して実行してみます。

![Pixel4 Emulator でアプリを開いたときの様子](https://storage.googleapis.com/zenn-user-upload/3ck4my6py4f4lra31h6ue20tiht5=300x)
***Pixel 4 API 30***

エミュレーター上でもエラーなく実行でき、Chrome で実行したときと同じような結果が得られました。このように Flutter は単一のコードで Web・モバイルを問わずに実行できる点が特徴です。

以上で、Scoop からインストールした Flutter、 Windows Package Manager でインストールした Android Studio を組み合わせて開発環境を構築することが出来ました。

# おわりに

今回、コマンドラインからパッケージ マネージャーを使って Android Studio と Flutter 、各種 SDK をインストールしてアプリをビルドするところまで行いました。Windows Package Manager はまだ現時点プレビュー版ですが、もうすぐ正式リリースされるみたいです。GUI を持つアプリケーションであっても、コマンドラインからいい感じにセットアップしてくれるのが便利だと感じました。また、Scoop も管理者権限をつかうことなく、アプリやコマンドをインストールできるので便利です。最終的には Windows Package Manager にパッケージ管理を集約し、コマンド 1 つで Windows の環境構築もできるようにしたいところです。

開発環境を構築する際に Mac の homebrew を使っているドキュメントや記事が多く、Windows での開発環境構築となると環境変数面などの操作がネックで億劫うになりがちでしたが Windows Package Manager や Scoop などのパッケージマネージャーを使うことで GUI をあまり使うことなく爆速で開発環境を構築を実現できて満足しています。

Windows 10 が登場してしばらく経ちますが、今現在も PowerToys、Windows Terminal の登場や、Windows Package Manager の提供により快適な環境ができてきているので今後どんな機能が提供されていくの期待したいですね。

長くなりましたが、最後まで読んでいただきありがとうございました。

# 参考ドキュメント

- [winget ツールを使用したアプリケーションのインストールと管理 | Microsoft Docs](https://docs.microsoft.com/ja-jp/windows/package-manager/winget/)
- [windows 10でFlutter開発環境構築【Scoop使用】 - Qiita](https://qiita.com/StrayDog/items/5ba0cbc00606eb8a0d46)
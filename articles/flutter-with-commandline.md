---
title: "Windowsのコマンドラインを駆使してFlutterの開発環境を構築してみた"
emoji: "🛫"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["flutter", "windows", "androidstudio", "scoop", "winget"]
published: true
---

## はじめに

Macで開発環境を構築する際にHomebrewなどのパッケージマネージャーを使って構築している記事をよく見かけます。パッケージマネージャーを使うことで、コマンドラインでパッケージの依存関係も解決してインストールできるため、開発環境構築の際に重宝されます。パッケージマネージャーを使うメリットに、コマンドのインストールと同時にパスも通してくれるためすぐにコマンドの実行ができること。コマンドのある場所をIDE（統合開発環境）などに教えてあげることですぐに開発に取りかかれるという点があります。

Windowsの場合はプログラミング言語やIDEもインストーラーを用いたインストールが多く、パスを通すために各自で直接システム環境変数を触るなどの作業が発生し少し手間になります。例えば、WindowsでJavaのソースをコンパイルをするために、JDKダウンロード → binディレクトリの場所（パス）をシステム環境変数に追加しパスを通す作業をした経験があるのではないでしょうか。

開発を始める前の環境構築で頻出するシステム環境変数、パス周りの概念がわからないと初学者にとって環境構築は少しハードルが高いように感じます。私がプログラミングを勉強し始めたとき、環境変数編集から管理者権限で間違えて他のシステム側の変数を削除してしまったためPCの起動ができなくなり、泣く泣くWindowsを再インストールしたことがあります。その経験から、Windows上でなるべくシステム環境変数を触らず、環境を汚すことなく手軽に開発環境を構築できればいいなと感じていました。そんなときに、Linuxのaptやyumといったパッケージマネージャーと出会い、その仕組みや手軽さに感動した記憶があります。

### Windowsにおけるパッケージマネージャー

Macには[Homebrew](https://brew.sh/index_ja)という強力なパッケージマネージャーが存在しますが、Windowsにも10年ぐらい前からパッケージ管理の仕組みをもつソフトがいくつか存在し、今現在も開発が続けられています。そして現在、Microsoftが[Windows Package Manager Client](https://github.com/microsoft/winget-cli) として公式のパッケージマネージャーを開発しており。**2021 年 4 月現在でプレビュー版**として提供しております。パッケージマネージャーを使うことでCLI（コマンドプロンプトなど）からパッケージインストールや依存関係の管理ができるため、Windowsでも柔軟な環境構築ができるような日も近いのではと感じます。

今回は、Windowsのパッケージマネージャである、[Scoop](https://scoop.sh)とWindows Package Manager Clientを使用してFlutterのアプリをビルド、実行できるところまでやってみます。

[Flutter の公式のドキュメント](https://flutter.dev/docs/get-started/install/windows)でも紹介されていますが、Flutterを公式サイトからダウンロードして、ローカルに配置、システム環境変数にFlutterのパスを追加する方法が一般的だと思われます。ですが、今回はなるべく環境変数を触らずにコマンドでパッケージマネージャーを操作してFlutterのアプリの編集と実行できる環境を構築することを目標に環境構築をしました。
### Flutterとは

https://flutter.dev/

Googleが開発しているUIツールキットになります。Dart言語で書かれており、単一のコードからモバイル・Web・デスクトップなどの環境を問わずに実行できるアプリを作成できる「クロスプラットフォーム開発」が特徴です。最近はモバイルアプリ開発を始める人がFlutterから学び始めている例を多く見るため、どんどん盛り上がってる印象を受けます。FlutterはSDKのインストールとプラグインのインストールを行うことでAndroidStudioやXcodeといったIDEを使用して開発することが出来ます。

## インストール検証環境

OSにはWindows10を使用しています。

| ツール | バージョン |
| ---- | ---- |
| Windows Package Manager | v0.2.10771 プレビュー |
| Scoop | 3d67b7d3 |
| Android Studio | 4.1.2.0 |
| Flutter | 2.0.4 |

## Windows Package Managerの準備

Windows Package Managerを使用してコマンドでAndroid Studioをインストールしていきます。
Windows Package Manager（winget-cli）はMicrosoft Storeからアプリインストーラーを更新することで使用できるようになります。

WindowsのスタートメニューからMicrosoft Storeを起動して「アプリ インストーラー」と検索するか、以下のリンクから飛べます。
https://www.microsoft.com/ja-jp/p/%E3%82%A2%E3%83%97%E3%83%AA-%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%A9%E3%83%BC/9nblggh4nns1?activetab=pivot:overviewtab

インストールページが開かれます。

![アプリインストーラーの詳細情報の画像](/images/flutter-with-commandline/image01.png)
*すでに更新されている場合はこのようになります*

インストールが終わったらPowerShellを**管理者権限で起動**します。
ターミナルに`winget`と入力するとコマンドの使用例とヘルプが表示されます。wingetとはWindows Package Managerを操作するためのコマンドです。

```powershell:powershell
> winget

Windows Package Manager v0.2.10771 プレビュー
Copyright (c) Microsoft Corporation. All rights reserved.

WinGet コマンド ライン ユーティリティを使用すると、コマンド ラインからアプリケーションやその他のパッケージをインストールできます。

etc ...
```

ここまで表示できたらWindows Package Managerのインストールが完了です。

最初に以下のコマンドでソースを最新に更新しておきます。

```powershell:powershell
> winget source update

すべてのソースを更新しています...
ソースを更新しています: winget...
完了
```

Wingetコマンドでインストールできるパッケージ一覧を出力してみます。

```powershell:powershell
> winget search
```

たくさんのアプリがずらっと表示されます。この一覧にあるアプリがインストール可能です。
この中からAndroid Studioを探してみます。`search`に続けてアプリ名を入力します。

```powershell:powershell
> winget search AndroidStudio

名前           ID                   バージョン
----------------------------------------------
Android Studio Google.AndroidStudio 4.1.2.0
```

Googleが提供する、Android Studioが見つかりました。それではインストールしていきます。

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

「インストールが完了しました」となればインストール成功です。うまくいかない場合は管理者権限にてコマンドを実行しているかを確認してください。

Windowsのスタートメニューを確認するとAndroid Studioがインストールているのが確認できます。

![スタートメニューからAndroid Studioを確認した様子](/images/flutter-with-commandline/image02.png)

公式サイトからインストーラーをダウンロードしてローカルで実行、GUIをポチポチとすることなくAndroid Stuidoをインストールできました。インストールに時間もそれほどかからないです。
## Flutterの導入

ここまでコマンドだけでAndroid Studioをインストールできました。続いてScoopというパッケージマネージャーを導入してFlutterと依存関係をまとめて導入します。
ScoopとはWindows向けのパッケージマネージャーの1つでパッケージ管理に管理者権限を使わないという特徴を持ちます。

Scoopはコマンドツール以外にもAndroid StudioやVSCodeようなGUIアプリケーションもインストールできます。しかし、Scoopを使用する場合、Windowsの想定するディレクトリ構成と異なるため不具合に繋がる可能性もあります。そのため私は現在GUIベースのアプリケーションをWindows Package Manager、CUIベースのツールをScoopと分けて管理しています。

【公式サイト】
https://scoop.sh/

【GitHub】
https://github.com/lukesampson/scoop

### Scoopのインストール

インストールするために以下のスクリプトをPowerShellから実行します。

```powershell:Powershell
$ iwr -useb get.scoop.sh | iex
```

うまく実行できないときは以下のコマンドでユーザーポリシーを緩めることでインストールできます。

```powershell:Powershell
$ Set-ExecutionPolicy RemoteSigned -scope CurrentUser
```

Scoopコマンドが実行できることを確認できればインストールは完了です。

```powershell:Powershell
$ scoop

Usage: scoop <command> [<args>]

...
```

コマンドの使い方と使用可能なオプションが表示されます。

### FlutterをScoopからインストール

Scoopコマンドが使えることを確認できたら、続けてFlutterを導入します。
Scoopには、**Bucket**という概念が存在し、Bucketを追加することでインストールできるアプリを増やすことができます。デフォルトでは`main Bucket`しか追加されていません。
Flutter関連のツールはextras Bucketとjava Bucketに存在するため、予め追加しないとインストールできません。

以下のコマンドでFlutterのインストールに必要なBucketをまとめて追加出来ます。

```powershell:powershell
 > scoop bucket add java extras versions
```

Bucketの追加が完了したら続けてFlutterの開発に必要なものを導入していきます。

以下のコマンドを実行します。

```powershell:powershell
> scoop install flutter
```

Flutterのインストールが始まります。インストール途中で以下が依存関係として順番にインストールされます。

- adb[64bit]
- android-sdk[64bit]
- adopt8-hotspot[64bit]
- flutter[64bit]

依存関係としてなにがインストールされるかは`depends`で都度確認できます。

```powershell:powershell
> scoop depends  flutter

adopt8-hotspot
android-sdk
adb
```

最後にAndroid SDKのライセンスが表示されるので、承諾していきます。

続けてインストールするSDKプラットフォームを選択します。デフォルトで一番最新のものが選択されます。こだわりがなければデフォルトで問題ありません。

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

再度ライセンスに承諾することでSDK Platformのインストールが行われます。

インストールが終了する際に`flutter doctor`が走り、状態をチェックしてくれます。これでFlutterのインストールは完了です。
この時点でFlutterにパスが通っているため、Windowsの環境変数の編集からパスを通す必要はありません。便利ですね。

しかし、doctorコマンドを実行したときにAndroid toolchainでエラーがでますので、Android Studio側で修正していきます。

## Android Studioの初期設定

最初に、WingetでインストールしたAndroid Studioを起動します。最初の設定でAndroid SDKが見つからないとエラーになります。

![Android SDKのエラー画面](/images/flutter-with-commandline/image03.png)

ここでAndroid SDKの場所を設定してあげます。
Scoopでインストールしたものは~\scoop\の中にすべてまとめられています。

Scoopでインストールしたアプリとライブラリの格納場所は以下のコマンドで調べることが出来ます。

```powershell:powershell
# インストール先を絶対パスで取得
> scoop prefix android-sdk

C:\Users\user\scoop\apps\android-sdk\current
```

Android SDK Location:に上のコマンドで取得したSDKへのフルパスを指定し続行します。

![Android SDKのパスを指定する様子](/images/flutter-with-commandline/image04.png)

パスを指定するとその他必要なコンポーネントのダウンロードが始まるので終わるまで待ちます。
Android Studioのメインメニューが表示されればセットアップは完了です。

SDKの場所のセットアップが終わった後に、PowerShellにてもう一度`flutter doctor`を実行します。

```powershell:powershell
> flutter doctor

Doctor summary (to see all details, run flutter doctor -v):
[!] Android toolchain - develop for Android devices (Android SDK version 30.0.3)
    ! Some Android licenses not accepted.  To resolve this, run: flutter doctor --android-licenses

```

上から2番目のAndroid toolchainの内容が変わっています。追加でライセンスに承諾する必要があるため、以下のコマンドでライセンスに承諾します。

```powershell:powershell
> flutter doctor --android-licenses
```

もう一度`flutter doctor`を実行して`• No issues found!`と出力されればFlutterの導入は完了です。

### Flutter開発プラグインの導入

Flutterのプロジェクトを作成するためにプラグインが必要になるためインストールします。
Android Studioのメニュー画面から「Configure」→「Plugins」と進みます。

![Pluginを選択する様子](/images/flutter-with-commandline/image05.png)

表示されるダイアログの中でタブを「Marketplace」に切り替えて「Flutter」と検索してインストールします。

![Flutterプラグインをインストールする様子](/images/flutter-with-commandline/image06.png)

これでFlutterの開発プラグインの導入は完了です。合わせて **Dartのプラグインも必要になるのですが Flutterのプラグインをインストールするときに合わせてインストールされる**ので気にしなくて問題ありません。

Android Studioを再起動すると、「Create New Flutter Project」の項目が増えているので、ここから新規Flutterプロジェクトを作成できるようになります。うまく反映されない場合はプラグイン画面から「Dart」と「Flutter」プラグインが**Enable**になっていることを再度確認してください。

![再起動したあとのメインメニュー](/images/flutter-with-commandline/image07.png)
*Create New Flutter Projectが追加された*

画面の指示に従ってプロジェクト作成をしていくのですが、途中でFlutter SDKのパスを指定する箇所があります。

![SDKへのパスを指定する画面の画像](/images/flutter-with-commandline/image08.png)
*デフォルトでSDKへのパスが指定されていないので入力*

インストールしたFlutter SDKのパスはAndroid SDKと同じように以下のコマンドで取得できます。

```powershell:powershell
# インストール先を絶対パスで取得
> scoop prefix android-sdk

C:\Users\user\scoop\apps\flutter\current
```

Flutter SDK pathのところにC:\Users\ユーザー名\scoop\app\scoop\apps\flutter\currentを指定して続行します。

そのまま続行すると新規FlutterプロジェクトがAndroid Studio上で開かれます。

## Flutterアプリを実行する

Android Studio上でプロジェクトが開かれるのを確認したら、実際に実行できるか試します。

上部のメニューからChrome（Dev）を選択して`Shift`＋`F10`で実行します。

![ChromeでFlutterが実行される様子](/images/flutter-with-commandline/image09.png)
*Flutter Demo Home Page*

自動的にChrome上でFlutterのデモアプリが起動しました。ホットリロードに対応しているためコードの変更がリアルタイムに反映されます。

AVDマネージャーからAndroid Emulatorを作成して実行してみます。

![Pixel4 Emulator でアプリを開いたときの様子](https://storage.googleapis.com/zenn-user-upload/3ck4my6py4f4lra31h6ue20tiht5=300x)
*Pixel 4 API 30*

エミュレーター上でもエラーなく実行でき、Chromeで実行したときと同じような結果が得られました。このようにFlutterは単一のコードでWeb・モバイルを問わずに実行できる点が特徴です。

以上で、ScoopからインストールしたFlutter、 Windows Package ManagerでインストールしたAndroid Studioを組み合わせて開発環境を構築することが出来ました。

## おわりに

今回、コマンドラインからパッケージ マネージャーを使ってAndroid StudioとFlutter 、各種SDKをインストールしてアプリをビルドするところまで行いました。Windows Package Managerはまだ現時点プレビュー版ですが、もうすぐ正式リリースされるみたいです。GUIを持つアプリケーションであっても、コマンドラインからいい感じにセットアップしてくれるのが便利だと感じました。また、Scoopも管理者権限をつかうことなく、アプリやコマンドをインストールできるので便利です。最終的にはWindows Package Managerにパッケージ管理を集約し、コマンド1つでWindowsの環境構築もできるようにしたいところです。

開発環境の際にHomebrewを使っているドキュメントが多く、Windowsで参考にするとつまづきがちでしたがパッケージマネージャーを使うことで詰まることなく爆速での環境構築ができて満足しています。

Windows10は今現在もPowerToys、Windows Terminalや、Windows Package Managerの登場により快適な環境ができてきているので今後さらに期待したいですね。

長くなりましたが、最後まで読んでいただきありがとうございました。

## 参考ドキュメント

- [winget ツールを使用したアプリケーションのインストールと管理 | Microsoft Docs](https://docs.microsoft.com/ja-jp/windows/package-manager/winget)
- [windows 10でFlutter開発環境構築【Scoop使用】 - Qiita](https://qiita.com/StrayDog/items/5ba0cbc00606eb8a0d46)

---
title: "Composerのcreate-projectコマンドは何をしているのか追ってみる"
emoji: "🎼"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["php", "composer"]
published: true
---

## はじめに

PHPでパッケージ間の依存解決をするときは[Composer](https://getcomposer.org)がよく使われます。PHPのフレームワークなどでプロジェクトの雛形を作ろときに毎回`composer create-project xxx`とお決まりのコマンドを実行することが多いです。

例えば、PHPのフレームワークで有名な[Laravel](https://laravel.com)の雛形を作成する場合、公式ドキュメントの冒頭にある以下のコマンドを実行すると最新バージョンのLaravelプロジェクトが自動生成されます。

```shell
$ composer create-project laravel/laravel example-app
```

コマンドを何気なくタイプしている中で具体的に何をしているのか気になったので実態を調べてみました。
コマンドの名前からしてJavaScriptのパッケージ管理ツールnpmやYarnでいう`npm init`や`yarn init`みたいなもののように感じますがどうなのでしょうか。

## 結論

`git clone`して`composer install`を順番に実行するのと同じ挙動をする。

予想よりも意外とシンプルで驚きました。続けて詳しく見ていきます。

また`composer create-project`の実装は以下のPHPファイルになります。
https://github.com/composer/composer/blob/main/src/Composer/Command/CreateProjectCommand.php

## create-projectコマンドの実態

Composer公式ドキュメントのcreate-project[^1]項を眺めていたら気になる一文を発見しました。

> You can use Composer to create new projects from an existing package. This is the equivalent of doing a Git clone/svn checkout followed by a composer install of the vendors.
[^1]: 引用 https://getcomposer.org/doc/03-cli.md#create-project

要約すると、GitやSvnなどのバージョン管理ツールでリポジトリをローカルにクローンして`composer install`することと同義であるとのこと。想像よりもシンプルで驚きました。

本当にそうなのか検証してみます。

## Laravelを例に検証する

以下、Laravelの例で考えます。

Laravelの実体は以下のGitHubリポジトリに存在します。
https://github.com/laravel/laravel

記事執筆時点で最新バージョンはv9.4.1となっております。このリポジトリをローカルにクローンするには以下のコマンドを実行します。

```shell
$ git clone https://github.com/laravel/laravel.git example-app
```

上のコマンドを実行することで現在のディレクトリ配下にexample-appというディレクトリが生成され、中にGitHubリポジトリ上のソースがすべてコピーされた状態になります。

この状態でLaravelのビルトインサーバー（組み込み開発サーバー）を起動しようとすると必要なパッケージをロードできないためarisanコマンドでPHPのWarningが発生します。

```shell
$ php artisan serve

PHP Warning: ..
```

なぜならGitHub上にあるlaravelのソースにはVendorディレクトリが含まれていないためです。そこでディレクトリルートにあるcompose.jsonの内容をもとに必要パッケージのインストールと依存解決をしてあげる必要があります。

その時に使われるのが`composer install`になります。

ここまで来るともうお分かりなのですが、`composer create-project laravel/laravel example-app`はlaravelのlaravelリポジトリを`git clone`したあとに`composer install`を実行してよしなに環境を構築してくれるワンライナーの役目を果たしているのでした。

`composer create-project`を使わずに、`git clone`、`composer install`で構築した場合は.envファイルが存在しないため500エラーになる可能性があります。その場合は、.envファイルを構成しencriptキーを生成することで解決します。

```shell
# サンプルの.envファイルをコピー
$ cp .env.example .env

# encriptキーを生成してビルトインサーバー起動
$ php artisan key:generate && php artisan serve
```

基本的には公式ドキュメントに合わせて`composer create-project`でプロジェクトを開始し適宜カスタマイズしていくのが無難そうです。

![ビルトインサーバートップページの画像](/images/what-is-composer-create-project/image01.png)

## 応用してみる

つまりコマンド内の`laravel/larvel`の部分を任意のリポジトリ名に変えて上げることで他のフレームワークにも応用できます。

例えばSymfonyというPHPフレームワークの場合は以下のようになります。

```shell
$ composer create-project symfony/skeleton example-app
```

コマンド1つで開発環境を作れるのはすごく便利ですね。

## おわりに

今回は手短ながら、分かっているようで分からない`composer create-project`についてまとめました。ドキュメントを読み解くこと発見も多くあり、より知識が深まるので気になったら調べてみると精神は大切ですね。

最後まで読んでいただきありがとうございました。

---
title: "composer create-projectコマンドは何をしているのか追ってみる"
emoji: "🎼"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["php", "composer"]
published: false
---


# はじめに

PHPでパッケージの依存解決をするときは[Composer](https://getcomposer.org)を使うことが多く、PHPのフレームワークなどでプロジェクトの雛形を作ろときに毎回`composer create-project xxx`とお決まりのコマンドを実行することが多いです。

たとえば、PHPアプリケーションフレームワークで有名な[Laravel](https://laravel.com)の雛形を作成する場合、公式ドキュメントの冒頭にある以下のコマンドをタイプすると最新バージョンのLaravelプロジェクトが自動的に生成されます。

```shell
$ composer create-project laravel/laravel example-app
```

コマンドを何気なくタイプしている中で具体的に何をしているのか気になったので実態を調べてみました。
コマンドの名前からしてJavaScriptのパッケージ管理ツールnpmやYarnでいう`npm init`や`yarn init`みたいなもののように感じますがどうなのでしょうか。

# 結論

`git clone`して`composer install`をワンライナーで実行するのと同じ挙動になる。

# create-projectコマンドの実態

Composer公式ドキュメントのcreate-project[^1]を眺めていたら気になる一文を発見しました。

> You can use Composer to create new projects from an existing package. This is the equivalent of doing a Git clone/svn checkout followed by a composer install of the vendors.

[^1]: 引用 https://getcomposer.org/doc/03-cli.md#create-project

要約すると、GitやSvnなどのバージョン管理ツールでリポジトリをローカルにクローンして`composer install`することと同義であるとのこと。想像よりもシンプルで驚きました。

本当にそうなのか検証してみます。

# Laravelを例に検証する

以下、Laravelの例で考えます。

Laravelの実体は以下のGitHubリポジトリに存在します。

https://github.com/laravel/laravel

記事執筆時点で最新バージョンはv9.4.1となっております。このリポジトリをローカルにクローンするには以下のコマンドを実行します。

```shell
$ git clone https://github.com/laravel/laravel.git example-app
```

上のコマンドを実行することで現在のディレクトリ配下にexample-appというディレクトリが生成され、中にGitHubリポジトリ上のソースがすべてコピーされた状態になります。

この状態でLaravelのローカルサーバーを起動しようとすると必要なパッケージをロードできないためarisanコマンドでPHPのWarningが発生します。

```shell
$ php artisan serve

PHP Warning: ..
```

なぜならGitHub上にあるlaravelのソースにはVendorディレクトリが含まれていないためです。そこでディレクトリルートにあるcompose.jsonの内容をもとに必要パッケージのインストールと依存解決をしてあげる必要があります。

その時に使われるのが`composer install`になります。

ここまで来るともうお分かりなのですが、`composer create-project laravel/laravel example-app`はlaravelのlaravelリポジトリを`git clone`したあとに`composer install`を実行してよしなに環境を構築してくれるワンライナーの役目を果たしているのでした。

# 応用

つまりコマンド内の`laravel/larvel`の部分を任意のリポジトリ名に変えて上げることで他のフレームワークにも応用できます。

例えばSymfonyというPHPフレームワークの場合は以下のようになります。

```shell
$ composer create-project symfony/skeleton example-app
```

コマンド1つで開発環境を作れるのはすごく便利ですね。

# おわりに

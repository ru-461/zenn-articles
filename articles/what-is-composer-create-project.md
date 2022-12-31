---
title: "composer create-projectコマンドは何をしているのか追ってみる"
emoji: "🎼"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["composer", "php"]
published: false
---


# はじめに

PHPでパッケージの依存解決をするときは[Composer](https://getcomposer.org)を使うことが多く、PHPのフレームワークなどでプロジェクトの雛形を作ろときに毎回`composer create-project xxx`とお決まりのコマンドを実行することが多いです。

たとえば、Laravelプロジェクト雛形を作成する場合、公式ドキュメントの冒頭にある以下のコマンドをタイプすると最新バージョンのLaravelプロジェクトが自動的に生成されます。

```shell
$ composer create-project laravel/laravel example-app
```

コマンドを何気なくタイプしている中で具体的に何をしているのか気になったので実態を調べてみました。
コマンドの名前からしてJavaScriptのパッケージ管理ツールnpmやyarnでいう`npm init`や`yarn init`みたいなもののように感じますがどうなのでしょうか。

# 結論

`git clone`して`composer install`をワンライナーで実行するのと同じ挙動になる。

# create-projectコマンドの実態

Composerの公式ドキュメントを眺めていたら気になる一文を発見しました。

| You can use Composer to create new projects from an existing package. This is the equivalent of doing a Git clone/svn checkout followed by a composer install of the vendors.

要約すると、Gitやsvnなどのバージョン管理ツールでリポジトリをローカルにクローンして`composer install`することと同義であるとのこと。

本当にそうなのか検証してみます。

# おわりに

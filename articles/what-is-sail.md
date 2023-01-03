---
title: "sailコマンドと仲良くなりたい"
emoji: "⛴️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["laravel", "laravelsail"]
published: false
---

# はじめに

Laravel Sailと呼ばれる公式が提供しているLaravelの開発環境があります。
内部的にdocker-composeを利用し、爆速でLaravelの開発環境を立ち上げられるかつ公式が提供しているということもあり利用者を広げています。

https://github.com/laravel/sail

Laravel SailはDocker初学者への配慮もあり、今自分が操作しているコンテナを意識することなくSailコマンドを経由してコマンド操作が可能となっております。
Laravelのビルトインサーバーを立ち上げるコマンド`php artisan serve`は`sail artisan serve`で代替されています。

このようにsailコマンドを経由することでコンテナを意識することなくコマンドの実行ができます。

Dockerで動いていることをあまり意識することなくコマンドを実行できるのは便利ですが、ふと`sail`の実装について少し気になったことがあり、調べてみました。

# おわりに

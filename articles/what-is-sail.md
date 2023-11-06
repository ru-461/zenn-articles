---
title: "Laravel SailのSailコマンドと仲良くなりたい"
emoji: "⛴️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["laravel", "laravelsail"]
published: true
---

## はじめに

[Laravel Sail](https://laravel.com/docs/9.x/sail)と呼ばれる公式が提供しているLaravelの開発環境があります。

内部的にDocker Composeを利用し、Dockerの利用環境があれば簡単に開発環境を用意できるという点、公式が提供している点が評価され日々利用者を広げています。
https://github.com/laravel/sail

Laravel SailはDocker初学者への配慮もあり、今自分が操作しているコンテナを意識することなくSailコマンドを経由してコマンド操作が可能となっております。そして公式のドキュメントには以下のような記載があります。

> without requiring prior Docker experience.[^1]

[^1]: https://laravel.com/docs/9.x/sail

和訳すると「Dockerの経験を必要とせず」となります。
Dockerの経験がなくても簡単にPHP・MySQL・Redisといった複数のサービスを組み合わせてLaravelの開発にすぐ着手させることをコンセプトに掲げています。

Sail専用のコマンドが用意されており、Laravelより学習コストが嵩むのではという懸念もありますが、個人的にさほど問題になることはないように感じました。

例えば、ビルトインサーバーを立ち上げるコマンド`php artisan serve`は`sail artisan serve`で代替されています。

このようにLaravel Sailが独自に用意している`sail`コマンドを経由することでコンテナを意識することなく開発ができます。

Docker Composeがブラックボックス化されていることでコンテナをあまり意識することなく開発を進められる点は便利ですが、ふと`sail`の実装について少し気になったことがあり、調べてみました。

## Sailコマンドをみてみよう

本項ではコマンドの実態についてソースを読んでいて気になった部分を見ていきます。

### コマンドの場所について

Sailコマンドの実態は約500行程のBashスクリプトになっており、「vendor/laravel/sail/bin/sail」に実装があります。

コマンドラインから呼び出す際、直接呼び出すのではなく、「vendor/bin/sail」を経由して呼び出すことを公式が推奨しており、冗長化防止のため以下のBashエイリアスを公式が提唱しています。

```shell:.bashrc
alias sail='[ -f sail ] && bash sail || bash vendor/bin/sail'
```

本記事内では上記のエイリアスを事前に設定している状態で進めますので予めご了承ください。

LaravelのGitHubリポジトリ上では以下のソースに該当します。

https://github.com/laravel/sail/blob/1.x/bin/sail

### ファイルの実行確認

中身を見ると分かるのですが、全体的にかなりシンプルな構成となっています。
基本的にコマンドラインから`vendor/bin/sail $1`の形で引数を付けて実行するたびにこのファイルの中が呼び出される仕組みになっています。

試しにスクリプトのファイルの先頭で定義済みの`UNAMEOUT`を出力して終了させてみます。
`UNAMEOUT`は現在実行しているOS名を取得して格納しているため、実行しているOSに応じて異なる値が入ります。
Sailコマンド内ではサポートOSの判定に使用しています。

```shell:vendor/laravel/sail/bin/sail
#!/usr/bin/env bash
UNAMEOUT="$(uname -s)"

echo ${UNAMEOUT};
exit 1;
```

ファイルを保存しコマンドラインから確認します。

```shell
$ sail
Darwin
```

現在実行しているOS名を取得できます。Sailコマンドの実態ファイルを呼び出される様子を確認できました。
## 気になったところ

### docker compose

Sailコマンドの核ともいえるDocker Composeの操作箇所について見ていきます。

シェルスクリプトを読んでいくとDocker Composeのコマンドを`DOCKER COMPOSE`変数に代入して引数に応じてコマンドを動的に生成 → 引数付与 → 実行というプロセスが踏まれています。

このとき気になったのが次の箇所です。

```shell:vendor/laravel/sail/bin/sail
# Define Docker Compose command prefix...
docker compose &> /dev/null
if [ $? == 0 ]; then
    DOCKER_COMPOSE=(docker compose)
else
    DOCKER_COMPOSE=(docker-compose)
fi
```

コマンドの存在を判定してそれぞれ変数に代入しているのですが、全く同じ変数に代入しているのが少し気になりました。

`docker compose`と`docker-compose`一見同じコマンドのように見えますが、これはれっきとした別コマンドになります。Docker CLIの`compose`か、`docker-compose`かの違いになるようです。

詳しくは以下のドキュメントにまとめられております。

https://docs.docker.jp/compose/index.html#compose-v2-docker-compose

コマンドの確認をして適切なコマンドを変数に割り当てており、`DOCKER_COMPOSE`変数をベースに引数を追加してDocker Composeが実行されます。

### コマンド作成

Dokerの実行環境チェックが続き、200行目付近からSailコマンドの引数処理が続きます。
sailコマンドに続く引数を`$1`・`$2`として順番に受け取り第一引数の値に応じてif文で実行コマンドを分岐しています。

例えばLaravelの開発で頻発する`artisan`コマンドについて見ていきます。PHPコマンドのチェックに始まり引数チェックをたどって行くと途中に`artisan`または`art`の引数で実行される部分が見つかりました。

```shell:vendor/laravel/sail/bin/sail
elif [ "$1" == "artisan" ] || [ "$1" == "art" ]; then
    shift 1

    if [ "$EXEC" == "yes" ]; then
        ARGS+=(exec -u sail)
        [ ! -t 0 ] && ARGS+=(-T)
        ARGS+=("$APP_SERVICE" php artisan "$@")
    else
        sail_is_not_running
    fi
```

要約すると、`EXEC`で実行可否フラグを確認し、実行可能であれば対象のコンテナ内で`php artisan`コマンドを実行するように命令を組み立てています。

組み立てたコマンドがどこで実行されているのかというと最終行に書いてありました。

```shell:vendor/laravel/sail/bin/sail
"${DOCKER_COMPOSE[@]}" "${ARGS[@]}"
```

if文で引数を判定、コマンド組み立て、最後に実行するというかなりシンプルなプロセスで驚きました。

### コンテナ名

コンテナをあまり意識することなくコマンドを組み立てて実行してくれる点は便利です。

1つ注意点を挙げるとすると`ARGS`に`docker compose`の引数が`+=`で文字列連結されていく中で登場する`APP_SERVICE`変数は`laravel.test`という値が直接格納されている点です。

デフォルトでサービス名が決め打ちされているため、`laravel.test`から任意のコンテナ名に変更した場合は.envファイルも適切に変更しておく必要があります。

Docker初学者としてはハードコーディングされていたほうが意識することを少なくできてメリットに感じることが多いです。しかし、Docker Composeについてカスタマイズをしようとしたときに少し躓く可能性があったため補足しました。

## 【Tips】実行コマンドを確認してから実行する

Sailが暗黙的にコマンドを生成し適切なコンテナでコマンドを実行してくれる点はとても便利です。
しかし`docker compose`が完全に隠蔽されていることで、基本的にどのようなコマンドが実行されているかを見る機会がないというのも少し気になりました。

そこでsailコマンドファイル「vendor/laravel/sail/bin/sail」の最終行手前に以下のような処理を挟むことで実行コマンドを確認してから実行可能になります。
`read`でユーザーに確認し、`y`と入力したときのみコマンドを実行するようにしてみました。

```shell:vendor/laravel/sail/bin/sail
echo "Command:";
echo "===========================================================";
echo "${DOCKER_COMPOSE[@]}" "${ARGS[@]}";
echo "===========================================================";
read -p "Do you want to execute the following command? (y/n)" YN

if [ "${YN}" != "y" ]; then
    echo "Command canceled."
    exit 1
fi
```

即興で作ったもので、少し無理矢理感は否めませんが、実行時にどのようなDocker Comoposeコマンドが暗黙的に作られて実行されているかを確認できるのは非常に便利です。

例えばMySQLコンテナに入り、ログインする`sail mysql`というコマンドがあります。実際に実行してみると以下のようになります。
`docker compose`から始まるコマンドを見ていくとコンテナ内でコマンドを実行する`docker compose exec`に続けて実行するサービス名、コマンドが列挙されているのが分かります。つまりSailコマンド内に定義されているMySQLユーザー情報変数を展開し、MySQLへログインを試行していることになります。

```shell
$ sail mysql
Command:
===========================================================
docker compose exec mysql bash -c MYSQL_PWD=${MYSQL_PASSWORD} mysql -u ${MYSQL_USER} ${MYSQL_DATABASE}
===========================================================
Do you want to execute the following command? (y/n)y
```

`y`とタイプすることでMySQLへの接続とログインが自動的に行われます。

```shell
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 11
Server version: 8.0.31 MySQL Community Server - GPL

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

Sailが事前に用意しているコマンドが具体的に何をしているのかを確認することでDocker Composeへの理解も深まります。
私はこの方法でLaravel SailがDocker Composeで内部的に行っていることを1つ1つ確認して覚えました（笑）

他にもSailが提供している便利なコマンドが多数あります。
`sail --help`で確認したコマンドを上記の方法で確認し、Docker Composeコマンドが裏側で動いている様子を見てみるのも面白いので試してみてください。

## おわりに

本記事内ではSailコマンドにて気になった部分を取り上げながら解説をしてみました。Dockerを使ったLaravelの開発に少しむずかしい印象を抱いており敬遠していた部分があったのですが、紐解いていくことで少しSailコマンドと仲良くなれた気がします。

Laravel Sailを使うことでDockerを詳しく知らなくても開発に参画しやすいと言うメリットもありますが、その反面カスタマイズする際にDockerを避けては通れないと個人的に感じております。1からLaravelを学びつつDockerを学ぶというのは大変ですが、実際にソースを読みつつ内部的にやっていることをデバッグしてみるだけでも理解が深まります。その点でもあえてLaravel Sailを触って見る価値はあるのかなとも感じました。

この記事を通して少しでもLaravel Sailへの知見が深まれば幸いです。

最後まで読んでいただきありがとうございました。

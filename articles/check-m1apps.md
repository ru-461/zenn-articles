---
title: "「このアプリM1 Macに最適化されてる？」を一瞬で確認する"
emoji: "🌱"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["mac","applesilicon"]
published: false
---

# はじめに

Apple M１ チップ(Apple Silicon M1)を搭載した Mac シリーズが発売されてしばらく経ちました。M1 Mac は `Applesilicon` と呼ばれ、`ARM アーキテクチャ`を採用しています。そのため、今まで使用されていた IntelCPU(x86)ベースのアプリとの互換性がなくなり、そのままでは使用できなくなりました。そこで Apple は `rosetta2` という変換ツールを用意しており、x64 ベースのアプリを M1 チップ上で変換し動作させることができます。M1Mac 発売当初は x6４ベースのアプリを変換していたアプリがほとんどでしたが、いまは各アプリがこぞって M1 チップ上でネイティブに動作する Applesilicon 対応ビルドやユニバーサルビルドをリリースし、今日までどんどん M1 Mac への対応が進んでいます。

先日、待望されていた Docker Desktop が v3.3.1 にて正式に `AppleSilicon へ対応`し、話題になりました。
https://www.docker.com/blog/released-docker-desktop-for-mac-apple-silicon/

まだまだ盛り上がりを見せている M1 Mac 対応ですが、まだ Rosseta２を使っているアプリも多く存在します。今日現在、Mac の中に M1 チップへネイティブ対応しているアプリと、変換して動かしている従来のアプリが混在する状態となっているなか、どのアプリが M1 チップに最適化されているのかを一瞬で確認する方法について紹介します。

# インストール済みのアプリから確認する

すでに Mac にインストールしているアプリの中で M1 チップに最適化されているのかを確認するにはアクティビティモニタから使用アーキテクチャを見ることで判別できます。アプリを起動している状態で `アクティビティモニタ.app` を実行します。
アーキテクチャの項目に `Apple` と表示されていれば M１Mac に最適化されているといえます。また従来のアーキテクチャ(x64)で動作しているアプリは `Intel` と表示されます。

これで 1 つづつ確認していくこともできるのですが、`iMobie M1 App Checker`というツールを使用することで、インストール済みアプリから一括で対応状況を確認できます。

https://www.imobie.com/m1-app-checker/

公式サイトから `「Free Download」` をクリックしてインストールできます。Homebrew を使用している場合は、ターミナルから以下のコマンドでインストールもできます。

```shell:Terminal
$ brew install --cask imobie-m1-app-checker
```

アプリを起動させるとこのような画面が表示されます。

![M1-App-Checkerが起動した様子](https://storage.googleapis.com/zenn-user-upload/0si9fjy3uizcqutzipl5u9wqmgs2)

アプリケーションがインストールされているディレクトリ（デフォルトでは/Applications）を確認して「チェック」をクリックします。するとチェック結果が表示されます。

![M1-App-Checkの結果画面](https://storage.googleapis.com/zenn-user-upload/myi4j4psazwex1sm3qdo6384ufmz)

アプリ名の右側に M1 Mac への対応状況が表示されます。この中で `Intel64` と表示されているものが Rosetta2 を使用して動作しているアプリ、AppleSilicon に未対応のアプリとなります。 このように一瞬で AppleSilicon に対応しているかしていないかをすぐに確認できます。

# すべてのアプリから対応状況を調べる

アプリの中にはアップデートで AppleSilicon に対応するものが多くあります。現在使用しているアプリが現行バージョンにて AppleSilicon に対応しているかしていないかを調べらために用意されたサイトがあるため紹介します。

上記の M1-App-Checker は公式サイトにて AppleSilicon に対応しているかどうかを調べるための早見表を提供しています。

https://www.imobie.jp/m1-app-checker/?ref=m1

サイトの中段にリストがあり検索ボックスからアプリの対応状況を調べることができます。ただ全体的にアプリの情報が少し乏しく、検索しても見つからないアプリもあります。そこで以下のサイトも合わせて参照することをおすすめします。

## Is Apple silicon ready？





# おわりに
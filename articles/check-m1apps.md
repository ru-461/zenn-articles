---
title: "「このアプリM1 Macに最適化されてる？」を一瞬で確認する"
emoji: "🌱"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["mac", "applesilicon"]
published: true
---

# はじめに

Apple M１ チップ（AppleSilicon M1）を搭載したMacシリーズが発売されてしばらく経ちました。AppleSilicon M1は`ARMアーキテクチャ`を採用しています。そのため、従来のモデルにて使用されていたIntelCPU向けのアプリ`（x64 アーキテクチャ）`との互換性がなくなり、そのまま動かすことができなくなりました。

そこでAppleは`Rosetta 2`という変換ツールを用意しており、使用することでx64ベースのアプリをM1チップ上で変換し動作させることを可能としています。M1チップ上でネイティブに動作する`AppleSilicon対応ビルド`や`ユニバーサルビルド`が増えてきており、今では多くのアプリにてAppleSiliconの恩恵を預かることができるようになりました。

そして先日、ずっと待望されていたDocker Desktopがv3.3.1にて正式に`AppleSiliconへ対応`し、話題になりました。

https://www.docker.com/blog/released-docker-desktop-for-mac-apple-silicon/

まだまだ盛り上がりを見せているM1 Macですが、まだRosseta２を使用しないと動作させられないアプリも存在します。そこでM1チップへネイティブ対応しているアプリと、対応していない従来のアプリが混在する状態となっているなか、どのアプリがM1チップに最適化されているのかを一瞬で確認する方法について紹介します。

# インストール済みのアプリから確認する

すでにMacにインストールしているアプリの中でM1チップに最適化されているのかはアクティビティモニタからアーキテクチャ表示を見ることで判別できます。調べたいアプリを起動している状態で`アクティビティモニタ.app`を実行します。
プロセスの中にあるアーキテクチャの項目に`Apple`と表示されていればそのアプリはM１Macに**最適化されている**といえます。また従来のアーキテクチャ（x64）で動作しているアプリは`Intel`と表示されます。

![プロセスを見て調べる様子](https://storage.googleapis.com/zenn-user-upload/tfsrasc20r67jzy1unzbkbbubgts)

***プロセスごとにアーキテクチャを確認できる***
これで1つづつ確認していくこともできるのですが、**アプリごとに起動させて調べるのはとても大変**です。そこで`iMobie M1 App Checker`というツールを使用することで、インストール済みアプリから一括で対応状況を確認できます。

https://www.imobie.com/m1-app-checker/

公式サイトから`「Free Download」`をクリックしてインストールできます。パッケージマネージャーとして`Homebrew`を導入している場合は、以下のコマンドでインストールできます。

```shell:Terminal
$ brew install --cask imobie-m1-app-checker
```

アプリを起動させるとこのような画面が表示されます。

![M1-App-Checkerが起動した様子](https://storage.googleapis.com/zenn-user-upload/0si9fjy3uizcqutzipl5u9wqmgs2)

アプリケーションがインストールされているディレクトリ（デフォルトでは/Applications）を確認して「チェック」をクリックします。するとチェック結果が表示されます。

![M1-App-Checkの結果画面](https://storage.googleapis.com/zenn-user-upload/myi4j4psazwex1sm3qdo6384ufmz)
*AppleSilicon・UniversalがM1 Macに最適化済み*

アプリ名の右側にM1 Macへの対応状況が表示されます。この中で`Intel 64`と表示されているものがRosetta2を使用して動作しているアプリ、AppleSiliconに未対応のアプリとなります。
このように一瞬でAppleSiliconに対応しているかしていないかをすぐに確認できます。

# すべてのアプリから対応状況を調べる

アプリの中にはアップデートでAppleSiliconに対応するものが多くあります。現在使用しているアプリが現行バージョンにてAppleSiliconに対応しているかしていないかを調べるときに活用できるサイトがいくつかあるため紹介します。

上記のM1-App-Checkerは公式サイトにてAppleSiliconに対応しているかどうかを調べるための早見表を提供しています。

https://www.imobie.jp/m1-app-checker/?ref=m1

![M1-App-Checkerが提供するリスト画像](https://storage.googleapis.com/zenn-user-upload/b73mxxgsgf4rhxda4g5q37e5lwbo)

サイトの中段にリストがあり検索ボックスからアプリのAppleSilicon対応状況を調べることができます。ただ全体的にアプリの情報が他のサイトに比べて乏しく、検索しても見つからないアプリもあります。そこで以下で紹介するサイトも合わせて参照することをおすすめします。アプリが見つからない場合はリクエストもできるようです。

## Does it ARM？

![Does it ARM?のページ画像](https://storage.googleapis.com/zenn-user-upload/0wwsb2h4po7g1kurx201gbyqscpz)

https://doesitarm.com/

アプリ名から対応状況を検索できるサイトになります。アプリごとに詳細を見ることができ、対応したバージョンやリリースノートへのリンクがまとめられています。また目的のアプリがAppleSiliconにネイティブ対応していない場合、対応したときに**メールにて更新情報を受け取る機能**もあり、いち早く対応状況を知りたいときに活用できそうです。

## Is Apple silicon ready？

![Is Apple silicon ready？のページ画像](https://storage.googleapis.com/zenn-user-upload/3j4hp9lazkmwehywuttx7zobzu2d)

https://isapplesiliconready.com/jp

こちらも同じくアプリの対応状況をまとめたリストを提供しています。こちらは、デフォルトで**日本語表示に対応**しております。探しているアプリが見つからない場合や、情報が古い場合は情報の更新を申し立てることができるようです。

# おわりに

今回、AppleSiliconへの対応状況をアプリごとに調べる方法についてまとめました。Mac発売当初に比べて数多くのアプリがM1チップに最適化、環境を問わずに実行できるユニバーサルビルド化されてきており、M1 Macだからあれができない、難しいといったことはかなり少なくなってきました。アプリがネイティブ対応することで全体的に大きくユーザー体験が向上するため、アプリ側が対応している場合は適切に判断した上で対応ビルドにアップデートをしていきたいものです。
今年に入ってVSCodeやDocker Desktopがネイティブ対応するなど嬉しい話題が続いているため、今後もどんどんAppleSiliconが発展していくことを期待したいですね。

最後まで読んでいただきありがとうございました。
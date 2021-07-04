---
title: "HomebrewでYarnが怒られた話"
emoji: "🤔"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["homebrew", "mac", "yarn", "nodejs"]
published: true
---

# はじめに

以前、M1 MacbookにてHomebrewを使い、anyenvとnodenvをインストールしてNode.jsを使った開発環境を構築しました。

パッケージマネージャにYarnを使おうとHomebrewからインストールしたのですが、`brew doctor`コマンドで見てもらったところ以下のようなWarningがでてきて焦りました。

```shell
$ brew doctor

Please note that these warnings are just used to help the Homebrew maintainers
with debugging if you file an issue. If everything you use Homebrew for is
working fine: please don't worry or file an issue; just ignore this. Thanks!

Warning: You have unlinked kegs in your Cellar.
Leaving kegs unlinked can lead to build-trouble and cause formulae that depend on
those kegs to fail to run properly once built. Run `brew link` on these:
  yarn
```

brew doctorコマンドを使用するとこのメッセージが表示されますが、問題なくYarnを使ったパッケージ管理はできます。
メッセージの種類も`Warning : `だったので致命的なエラーではないという判断で今日まで放置していましたが、コマンドを実行するたびに表示されるのが少し気になり精神衛生上あまり良くないので解決策を探してみました。

# 環境・バージョン等

- macOS Big Sur 11.2.1
- HomeBrew 3.0.2
- anyenv 1.1.2
- nodenv 1.4.0+3.631d0b6
- Node.js 14.15.4
- yarn 1.22.10

# ここまでやったこと

https://github.com/anyenv/anyenv#homebrew-for-macos-user

上記を参考にHomebrewでanyenvをインストールし、anyenv経由でnodenvをインストール、Node.js 14.15.4（LTS）をグローバルとローカルの両方に指定しました。

当初はnpmを使って`Yarn`をインストールしようとしてましたが、[catnoseさんのこちらの記事](https://zenn.dev/catnose99/articles/9356979accca26)でM1 MacのHomebrewを使ってYarnをインストールできそうだったためHomebrewを使ってYarnをインストールすることにしました。

```shell
# Homebrew経由でYarnをインストール
$ brew install yarn

# バージョン確認
$ yarn --version
1.22.10
```

インストールに成功し、Yarnを使ったパッケージの管理はエラーなく行えるが、brew doctorを実行するたび毎回Warningが表示されるようになりました。

# 解決策

結論としては、すでにYarnが存在しており、シンボリックリンクが切れていることが原因でした。

エラーを見てみると、一番上に`You have unlinked kegs in your Cellar.`とあり、どうやらリンクが上手くできないために表示されているようです。
`Cellar`とは貯蔵庫を意味し、Homebrewではコマンドの実体（Keg）を格納するためのディレクトリを指しています。

Celllarディレクトリの場所は以下のコマンドで調べることができます。

```shell
$ brew --cellar

/opt/homebrew/Cellar
```

メッセージにてリンクしろと言われていたためbrew linkコマンドでリンクを試みるも。
```shell
$ brew link yarn

Linking /opt/homebrew/Cellar/yarn/1.22.10...
Error: Could not symlink bin/yarn
Target /opt/homebrew/bin/yarn
already exists. You may want to remove it:
  rm '/opt/homebrew/bin/yarn'

To force the link and overwrite all conflicting files:
  brew link --overwrite yarn

To list all files that would be deleted:
  brew link --overwrite --dry-run yarn
```

どうやらすでにYarnが /opt/homebrew/bin配下に存在しておりシンボリックリンクに失敗しているようです。

```shell
$ rm `/opt/homebrew/bin/yarn`
```

でディレクトリを削除して、競合するファイルを上書きしながらリンクを行いました。

```shell
$ brew link --overwrite yarn

Linking /opt/homebrew/Cellar/yarn/1.22.10... 2 symlinks created.
```

これで上手くシンボリックリンクが作成されたようです。

```shell
$ brew doctor

Your system is ready to brew.
```

Yarnに対するWarningがきれいさっぱりなくなりました。

# おわりに

Homebrewを使ってまだ日が浅いため、わからないことが多くありましたが、無事に問題を解決できてよかったです。
Homebrewはbrew doctorコマンドで起こっている問題をわかりやすく提示してくれるのがいいですね。

結果的に今回のエラーがなぜ起こっていたのかわかりませんでしたが、ずっと放置していた警告メッセージがなくなり今後ぐっすり眠れそうです。
エラーメッセージをしっかりと読み対応することが以下に大切かを感じさせられます。
今回の問題はYarn以外でも起こり得るようなので、同じような問題を抱えている方の参考になれば幸いです。

最後まで読んでいただきありがとうございました。

# 参考

- [brew doctorコマンドを実行した時に発生したwarningを1個ずつ解決していきます！ | LaptrinhX](https://laptrinhx.com/brew-doctorkomandowo-shi-xingshita-shini-fa-shengshitawarningwo1gezutsu-jie-jueshiteikimasu-2122113170/)
- [HomebrewでdoctorしたらWarning: You have unlinked kegs in your Cellarとなった時の対応方法 - Qiita](https://qiita.com/ponsuke0531/items/80f716c803ac23c7849d)
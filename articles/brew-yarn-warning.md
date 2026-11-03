---
title: "HomebrewでYarnが怒られた話"
emoji: "🤔"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["homebrew", "mac", "yarn", "nodejs"]
published: true
---

## はじめに

以前、Homebrewを使用して、anyenvとnodenvを使ったNode.jsの開発環境を構築しました。
パッケージマネージャであるYarnを使おうとHomebrewからインストールしたのですが、`brew doctor`を実行したところ以下の`Warning:`が出力され焦りました。

```shell
brew doctor
```

```text:実行結果
Please note that these warnings are just used to help the Homebrew maintainers
with debugging if you file an issue. If everything you use Homebrew for is
working fine: please don't worry or file an issue; just ignore this. Thanks!

Warning: You have unlinked kegs in your Cellar.
Leaving kegs unlinked can lead to build-trouble and cause formulae that depend on
those kegs to fail to run properly once built. Run `brew link` on these:
  yarn
```

使用上あまり問題はありませんが、毎回表示されるのは精神衛生上あまり良くないので解決策を探してみました。この記事は解決の備忘録となります。

## 環境・バージョン等

- macOS Big Sur 11.2.1
- Homebrew 3.0.2
- anyenv 1.1.2
- nodenv 1.4.0+3.631d0b6
- Node.js 14.15.4
- Yarn 1.22.10

## ここまでやったこと

https://github.com/anyenv/anyenv#homebrew-for-macos-user

上記を参考にHomebrewでanyenvをインストール、anyenv経由でnodenvをインストールしました。
Yarnはnpm経由ではなく、Homebrew経由でインストールしています。

```shell
# Homebrew経由でYarnをインストール
brew install yarn

# バージョン確認
yarn --version
```

```text:実行結果
1.22.10
```

無事にYarnのインストールに成功し、パッケージの管理を行えるようになりましたが、`brew doctor`を実行するたびに、上記のWarningが表示されるようになりました。

## 解決策

結論としては、すでにYarnが存在しており、シンボリックリンクが切れていることが原因でした。

エラーを見てみると、一番上に`You have unlinked kegs in your Cellar.`とあり、どうやらリンクが上手くできないために表示されているようです。
Cellarとは英語で「貯蔵庫」を意味し、Homebrewではコマンドの実体を格納するためのディレクトリを意味します。

Cellarの場所は以下のコマンドで調べることができます。

```shell
brew --cellar
```

```text:実行結果
/opt/homebrew/Cellar
```

Warning内のメッセージに従い、`brew link`を試みましたが、以下のエラーが出ました。

```shell
brew link yarn
```

```text:実行結果
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

どうやら、シンボリックリンクの作成に失敗しているようです。

```shell
rm /opt/homebrew/bin/yarn
```

メッセージに従い`/opt/homebrew/bin/yarn`を削除して、競合するファイルを上書きしつつリンクを行いました。

```shell
brew link --overwrite yarn
```

```text:実行結果
Linking /opt/homebrew/Cellar/yarn/1.22.10... 2 symlinks created.
```

2つのシンボリックリンクが作成されました。これで上手くシンボリックリンクが作成されたようです。
もう一度`brew doctor`を実行し、問題がないか診てもらいましょう。

```shell
brew doctor
```

```text:実行結果
Your system is ready to brew.
```

Warningがきれいさっぱりなくなりました。

## おわりに

Homebrewを使ってまだ日が浅いため、わからないことが多くありましたが、無事に問題を解決できてよかったです。Homebrewは`brew doctor`で問題をわかりやすく提示してくれるのがいいですね。
結果的に今回のエラーがなぜ起こっていたのかわかりませんでしたが、ずっと放置していた警告メッセージがなくなり今後ぐっすり眠れそうです。

同じような問題を抱えている方の参考になれば幸いです。

最後まで読んでいただきありがとうございました。

## 参考

[HomebrewでdoctorしたらWarning: You have unlinked kegs in your Cellarとなった時の対応方法 - Qiita](https://qiita.com/ponsuke0531/items/80f716c803ac23c7849d)

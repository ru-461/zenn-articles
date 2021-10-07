---
title: "PowerShellに導入したStarshipを最新にアップデートする"
emoji: "✨"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["powershell", "starship","scoop"]
published: false
---

# はじめに

私は普段からWindowsのWSL、PowerShellなどでRust製のプロンプト[Starship](https://starship.rs/ja-JP/)を愛用しています。

Starshipについては以前Zennにて詳しく[Rust製ツールでおしゃれなターミナル環境を作る【Starship ✕ exa】](https://zenn.dev/ryuu/articles/customize-your-terminal)というタイトルで紹介記事を書いたのでStarshipを知らない方はこちらから記事を覗いてもらえると嬉しいです。

Starshipを初めて導入したときはバージョン0.51.0だったのですが、あれから何回かアップデートがあり現在バージョン0.58.0となっています。

PowerShell以外のターミナルについてはHomebrewやaptでインストールしていたため半自動的に最新に更新されていたのですが、久しぶりにPowerShellを確認したところバージョンが0.51.0で止まっていたため手動でアップデートしてみました。

![WSL2とPowerShellの比較画像](/images/update-starship-in-powershell/image01.png)
*左がWSL2・右がPowerShell　それぞれStarshipのバージョンを表示しています*

# アップデートする

アップデートする際は至ってシンプルかつ簡単です。公式のドキュメントにもあるのですが以下のコマンドを実行するだけで最新のStarshipに置き換わります。

```shell
> sh -c "$(curl -fsSL https://starship.rs/install.sh)"
```

インストールもアップデートも同じコマンドでできるのは便利です。

このとき設定等がデフォルトの値に上書きされ、また最初から環境構築しなきゃならないのかと感じますが、アップデートの際は設定ファイルの互換性を保ったまま本体だけアップデートしてくれます。設定ファイル等の心配が減るだけでもかなり便利ですね。パッケージマネージャでバージョン管理をしている場合はあまり意識することがありませんが、手動でインストールする際の心理的負担が減るのは大きいです。

Starshipはクロスプラットフォーム対応していることもあり、それぞれの端末で環境を揃えたい場合に設定ファイルの互換性を考慮されていることが何よりも嬉しくなるポイントですね。

手動でインストールしている場合は手動で上のコマンドを実行しないと最新のバージョンになりません。
手動でアップデートを確認するのは少し不便なので、この際にパッケージマネージャでの管理に切り替えてみます。

# Scoopでの管理に乗り換える

手動での管理からScoopというWindows用のパッケージマネージャを使用したインストールに切り替えます。
ScoopはWindowsで使用できるコマンドラインベースのパッケージマネージャです。

https://scoop.sh

Windowsのパッケージマネージャというと[Chocolatey](https://chocolatey.org/)が有名ですが、Scoopも並んで人気があるツールです。

## Scoopのインストール

PowerShellにて以下のコマンドを順番に実行するだけでインストールが完了します。

```powershell
# ユーザーポリシーの変更
> Set-ExecutionPolicy RemoteSigned -scope CurrentUser

# Scoop本体のインストール
> iwr -useb get.scoop.sh | iex
```

インストールが完了後`scoop --version`と実行してバージョン情報が返ってくれば導入は成功です。
 ## Scoopを利用したStarshipのインストール

coopが導入できたらScoop経由でStarshipを導入します。Scoopがインストールされている状態で以下のコマンドを実行することでインストールできます。


```powershell
> scoop install starship
```

`'starship' was installed successfully!`と出力されると完了です。コマンドの最後に表示されるUsageに有効化する方法が記載されているのでガイドにしたがって設定します。

Starshipを有効化するためにはプロファイルに初期化処理を書く必要があります。プロファイルの場所は以下のコマンドで調べることができます。

```powershell
# プロファイルの場所を表示
> $PROFILE
```

上のコマンドで表示されたのが読み込んでいるプロファイルの絶対パスになります。メモ帳やVSCodeなどのエディタでプロファイルを開き最後の行に初期化処理を追記します。

```plaintext:Microsoft.PowerShell_profile.ps1
Invoke-Expression (&starship init powershell)
```

プロファイルの編集が完了したらリロードするためPowerShellを再起動します。再起動せずに`$PROFILE`と実行することでもプロファイルを再読み込みできます。

![WSL2とPowerShellの比較画像](/images/update-starship-in-powershell/image02.png)
*Scoop経由で最新のバージョンのStarshipがインストール完了*

これでStarshipをScoopで管理するようにできました。Starshipのアップデートがあった際は、`scoop update starship`のコマンドでバージョンが上げられるようになります。

# おわりに
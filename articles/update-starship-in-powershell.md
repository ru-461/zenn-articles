---
title: "PowerShellに導入したStarshipを最新にアップデートする"
emoji: "✨"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["powershell", "starship"]
published: false
---

# はじめに

私は普段からWindowsのWSL、PowerShellなどでRust製のプロンプト[Starship](https://starship.rs/ja-JP/)を愛用しています。

Starshipについては以前Zennにて詳しく[Rust製ツールでおしゃれなターミナル環境を作る【Starship ✕ exa】](https://zenn.dev/ryuu/articles/customize-your-terminal)というタイトルで紹介記事を書いたのでStarshipを知らない方はこちらから記事を覗いてもらえると嬉しいです。

Starshipを初めて導入したときはバージョン0.51.0だったのですが、あれから何回かアップデートがあり現在バージョン0.58.0となっています。

PowerShell以外のターミナルについてはHomebrewやaptでインストールしていたため半自動的に最新に更新されていたのですが、久しぶりにPowerShellを確認したところバージョンが0.51.0で止まっていたため手動でアップデートしてみました。

![WSL2とPowerShellの比較画像](/images/update-starship-in-powershell/image01.png)
*左がWSL2・右がPowerShell　それぞれStarshipのバージョンを表示しています*

# おわりに
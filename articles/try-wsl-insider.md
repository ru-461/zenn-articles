---
title: "Macに入れたWindows arm 64 InsiderPreviewでWSL2を動かす"
emoji: "🐴"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["windows","mac","wsl","parallels","arm"]
published: false
---

# はじめに

Mac 上で Windows を動かすといったことが以前の Intel 製 CPU を搭載していた Mac では用意にできていましたが 2020 年秋に登場した Mac シリーズは CPU が AppleSilicon に変わりそれとともにアーキテクチャが arm64 に変わりました。その影響で Windows を Mac 上で動かすのが困難になり、今日まで今現在今現在 ParallelsDesktop がバージョン 16.5 にて正式に AppleSilicon に対応しました。ちょうど、Windows も arm64 Insider Preview を Windows InsidersProgram で配布しており AppleSilicon の Mac で Preview 版の Windows が動作する段階まで来ています。

arm 版 Windows が一般的にライセンスが使用できるようになるかはまだ定かではありませんが、現状 Preview 版の Windows で WSL2 をどこまで使えるのかを試してみました。

# 検証環境

- macOS BigSur 11.3 (ホスト OS)
- Parallels Desktop 16 for Mac バージョン 16.5.0
- Windows 10 on ARM Insider Preview OS ビルド 21354.1 (ゲスト OS)

# おわりに
---
title: "npmは本当に「Node Package Manager」の意味なのか"
emoji: "🤔"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["npm"]
published: false
---

# はじめに

Node.jsにはデフォルトでnpmというパッケージマネージャが付属します。

npmはよく「Node Package Managerの略です」といった説明がされています。
一般的に「Node Package Manager」の頭文字を並べたものという認識がされているなか、先日npm-cliのGitHubリポジトリを見ていたときに興味深い説明を発見したので調べてみました。またこのような略式やその文化についても掘り下げてみます。

こちらがnpmのGitHubリポジトリになります。

https://github.com/npm/cli

# npmの主張

リポジトリのREADMEのなかに以下の見出しがあり、なぜnpmがnpmと呼ばれるのかについての注釈がありました。

以下、README内「FAQ on Branding」からの引用となります。

> Is "npm" an acronym for "Node Package Manager"?

> Contrary to popular belief, npm is not in fact an acronym for "Node Package Manager"; It is a recursive bacronymic abbreviation for "npm is not an acronym" (if the project was named "ninaa", then it would be an acronym).

日本語に訳すと以下のようになります。

Q. "npm "は "Node Package Manager "の頭文字ですか。

A. 一般的に信じられていることとは異なり、npmは実際には「Node Package Manager」の頭文字ではありません。

早速、「Node Package Manager」の頭文字を並べたもの ≠ npmと主張しています。さっそく否定から入ったのでこれには驚きました。ずっと頭文字を並べたものだと思っていたので公式リポジトリ内で弁明していることがとても衝撃的でした。

つまりnpmとは。
> npm is not an acronym（npmはacronymではない）

を再帰的に表現したものになりますね。

要するに、npnは「バクロニム」だとのことです。バクロニム？　やや少し難しい言葉がでてきましたね。

# バクロニムとは

バクロニム（backronym）は日本語で逆頭字語と表されます。日本で馴染みのある「あいうえお作文」のようなもので、深い意味はないけど英語の並べたようなものということです。

# 頭文字と再帰的頭字語の違いとは

頭文字は日本で良くみる略式になります。

それと対象的な位置にあるのが「再帰的頭字語」になります。たとえば、PHPやYAML、GNUといった単語は再帰的頭文字にあたります。

これは海外で一時期、略語でありながら皮肉、自虐的に略するような文化がありました。

# おわりに

今回はnpmのもつ意味や由来について調べてまとめてみました。

再帰的略語が飛び交うIT業界で仕事をしていると、なぜこのような名前にしたのか不思議に感じる技術を目にすることが多くあります。実際何気なく見ているものでも調べてみるとその背景には開発者の思いや、考え方が現れており、多くの発見があるように感じました。

また調べることで得られる発見により技術がもっと好きになりますね。

最後まで読んでいただきありがとうございました。
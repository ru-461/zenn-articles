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
一般的に「Node Package Manager」の頭文字を並べたものという認識がされているなか、先日npm-cliのGitHubリポジトリを見ていたときに興味深い説明を発見したので調べてみました。

こちらがnpmのGitHubリポジトリになります。

https://github.com/npm/cli

結論からまとめると**npmとは「npm is not an acronym」のバクロニム**であり、Node Package Managerを直接表すものではないとのことです。これだけ聞いてもあまりピンとこないので、この記事で略式やその文化についても掘り下げてみます。

またnpmといえば[npm.jsのWebサイト](https://www.npmjs.com/)の左上にnpmという文字列から3語の造語を生成して並べるトリックが有名です。

 10回クリックすることでGitHubのページへ遷移するようになっています。これもnpmの由来、意味に関係するのでしょうか。

# npmの主張

リポジトリのREADMEに以下の見出しがあり、なぜnpmがnpmと呼ばれるのかについての注釈がありました。

以下、README内「FAQ on Branding」からの引用となります。

> Is "npm" an acronym for "Node Package Manager"?

> Contrary to popular belief, npm is not in fact an acronym for "Node Package Manager"; It is a recursive bacronymic abbreviation for "npm is not an acronym" (if the project was named "ninaa", then it would be an acronym).

日本語に訳すと以下のようになります。

Q. "npm "は "Node Package Manager "の頭文字ですか。

A. 一般的に信じられていることとは異なり、npmは実際には「Node Package Manager」の頭文字ではありません。

早速、「Node Package Manager」の頭文字を並べたもの ≠ npmと主張しています。さっそく否定から入ったのでこれには驚きました。ずっと頭文字を並べたものだと思っていたので公式リポジトリ内で弁明していることがとても衝撃的でした。

要約するとnpmとは。
> npm is not an acronym（npmはacronymではない）

を再帰的に表現したものなんですね。

要するに、npnは「バクロニム」だとのことです。バクロニムというやや少し難しい言葉がでてきましたね。

# バクロニムとは

バクロニム（backronym）は日本語で逆頭字語と表されます。日本で馴染みのある「あいうえお作文」のようなもので、深い意味はないけど英語の並べたようなものということです。

# 頭文字と再帰的頭字語の違いとは

頭文字は日本で良くみる略式になります。

それと対象的な位置にあるのが「再帰的頭字語」になります。たとえば、PHPやYAML、GNUといった単語は再帰的頭文字にあたります。

これは海外で一時期、略語でありながら皮肉、自虐的に略するような文化がありました。

# npmの本質とは

FAQには合わせて以下のような説明があります。

>  The precursor to npm was actually a bash utility named "pm", which was the shortform name of "pkgmakeinst" - a bash function that installed various things on various platforms. If npm were to ever have been considered an acronym, it would be as "node pm" or, potentially "new pm".

> npmの前身は、実際には "pm "という名前のbashユーティリティで、これは "pkgmakeinst "というbash関数の短縮形の名前でした。もしnpmの頭文字を取るとしたら、"node pm "か、あるいは "new pm "になるでしょう。

npmの前身となるパッケージマネージャの名前が「pm」というものだったようです。npmを頭文字をとったものとするならnpmは「node pm」「new pm」のようになると主張しています。

このことから今はNode.jsに限らす、JavaScriptのパッケージマネージャとしての立ち位置にあるnpmがNodeに縛られないパッケージマネージャなんだと主張しているようち感じられます。このように見ていくとnpmの頭文字じゃないを再帰的に略したものがnpmであるという説明もやや皮肉が含まれていて面白いですね。

また、冒頭で紹介したnpmのトリックなのですが、10回クリックして遷移するGitHubリポジトリ内にもnpmはなぜnpmなのかについての説明があります。
# おわりに

今回はnpmのもつ意味や由来について調べてまとめてみました。今までただ、Node Package Managerの頭文字を取ったものだと認識していましたが、ただそれだけでなく否定的な意味合いもあり面白いと感じました。

再帰的略語が飛び交うIT業界で仕事をしていると、なぜこのような名前にしたのか不思議に感じる技術を目にすることが多くあります。実際何気なく見ているものでも調べてみるとその背景には開発者の思いや、考え方が現れており、多くの発見があるように感じます。

また気になったことを調べることで得られる発見により技術がもっと好きになりますね。

最後まで読んでいただきありがとうございました。
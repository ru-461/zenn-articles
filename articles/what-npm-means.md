---
title: "npmは本当に「Node Package Manager」の意味なのか"
emoji: "🤔"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["npm"]
published: false
---

# はじめに

Node.jsにはデフォルトでnpmというパッケージマネージャが付属します。

npmはよく「Node Package Managerの略です」といった説明がされています。しかしそれは本当に正しいのでしょうか。
一般的に「Node Package Manager」の頭文字を並べたものという認識がされているなか、先日npm-cliのGitHubリポジトリを見ていたときに興味深い説明を発見したので調べてみました。

こちらがnpmのGitHubリポジトリになります。

https://github.com/npm/cli

結論からまとめると**npmとは「npm is not an acronym」を再帰的に表したもの**であり、「Node Package Manager」の頭文字を表しているものではないとのことです。

https://twitter.com/npmjs/status/105690425242820608?s=20

これだけ聞いてもあまりピンとこないので、この記事でnpmの主張や技術名の略式にまつわる文化についても掘り下げてみます。

またnpmといえばのWebサイト[npm.js](https://www.npmjs.com/)の左上にnpmという文字列から3語の造語（バクロニム）を表示するトリックが有名です。

![npmトップページのIF](/images/what-npm-means/image01.gif)
***クリックのたびにnpmのバクロニムが次々に変化します***

 10回クリックすることで意味深なGitHubのリポジトリページへ遷移する仕様となっています。
 これもnpmの由来、意味に関係するのでしょうか。

# npmの主張

GitHubリポジトリのREADMEに以下の見出しがあり、なぜnpmがnpmと呼ばれるのかについての注釈がありました。

以下、README内「FAQ on Branding」からの引用となります。

> Is "npm" an acronym for "Node Package Manager"?

> Contrary to popular belief, npm is not in fact an acronym for "Node Package Manager"; It is a recursive bacronymic abbreviation for "npm is not an acronym" (if the project was named "ninaa", then it would be an acronym).

日本語に訳すと以下のようになります。

Q. "npm "は "Node Package Manager "の頭文字ですか。

A. 一般的に信じられていることとは異なり、npmは実際には「Node Package Manager」の頭文字ではありません。もしプロジェク名が「ninaa」なら頭文字といえます。

早速、「Node Package Manager」の頭文字を並べたもの ≠ npmと主張しています。さっそく否定から入ったのでこれには驚きました。ずっと頭文字を並べたものだと思っていたので公式リポジトリ内で主張していることがとても衝撃的でした。

要約するとnpmとは。

> npm is not an acronym（npmはacronymではない）

を再帰的に表現したものなんですね。

要するに、npmはNode package Managerという意味であるが、必ずしもnpnはそのまま頭文字を並べたものではないとのことです。

再帰的に表現や頭文字といった言葉が続きます。混乱を防ぐために、ここで一旦アクロニムという言葉やバクロニムといった略式、文化について整理していきます。

# バクロニムとアクロニムとは

バクロニム（backronym）は日本語で**逆頭字語**と表されます。また**頭文字**のことを英語でアクロニム（acronym）といいます。言葉だけだと少しわかりにくいので例を挙げてみます。

バクロニムの有名な例として「SOS」というものがあります。本来モールス信号であるものが（**S**ave **O**ur **S**hip）や（**S**ave **O**ur **S**ouls）といった後付の解釈によって表されます。本来意味のない言葉が後付によって頭文字のような意味をもつ。つまり一種の言葉遊びのようなものですね。日本だと「あいうえお作文」がバクロニムと言えそうです。

また。頭文字（アクロニム）は比較的イメージしやすいです。複数の単語から成り立つ言葉があったときにそれぞれの頭文字を並べたものになります。バクロニムの逆といった感じでしょうか。

普段目にする頭文字の例として、以下のようなものがあります。

- Suica（**S**uper **U**rban **I**ntelligent **CA**rd）
- UFO（**U**nidentified **F**lying **O**bject）

上のようにはそのまま英単語として発音できるものを頭文字（アクロニム）と呼びます。
中でも以下の例はどれも単語をアルファベット読みするため、アクロニムの中でも**イニシャリズム**と区別して呼ばれます。

- IT（**I**nformation **T**echnology）
- RPA（**R**obotic **P**rocess **A**utomation）
- IOC（**I**nternational **O**lympic **C**ommittee）

イニシャリズムのなかにアクロニムがふくまれるといったイメージだとわかりやすいです。多くの場面ではこれらをひとまとめに頭文字と表すことがあることもあります。

npmも「**N**ode **P**ackage **M**anager」かと思われがちですが、これが面白いことにnpmはバクロニムで「npm is not an acronym」の再起表現であると主張しています。これが本記事で取り上げる内容に繋がります。

# 頭文字と再帰的頭字語の違いとは

頭文字は日本で良くみる略式になりますが、それと対象的な位置にあるのが再帰的頭字語（recursive acronym）というものです。

これは海外で一時期、略語でありながら皮肉、自虐的に略するような文化があったことに由來します。

再帰的頭字語の例だとプログラミング言語のPHPの由来が有名です。
[PHPのドキュメントの冒頭](https://www.php.net/manual/ja/intro-whatis.php)に以下の説明があります（PHPマニュアルはじめにから引用）

> PHP (PHP: Hypertext Preprocessor を再帰的に略したものです) は、広く使われているオープンソースの汎用スクリプト言語です。

PHPはPHP: Hypertext Preprocessorを再帰的に略したものという説明をみて違和感を感じる人も少なくないです。頭文字をならべる感覚（アクロニム）で見ていくと、PHPのHPは直感的にわかるけど、先頭にあるPは一体何なんだろうという考えになり、ループに陥る感覚を覚えます。このような言葉を再帰的頭字語（recursive acronym）と呼びます。

再帰的頭字語の有名なものとしてYAMLやGNU、LAMEがあります。

- YAML（YAML Ain't a Markup Language）YAMLはマークアップ言語じゃないの意
- GNU（GNU's Not UNIX！）GNUはUNIXではないの意
- LAME（LAME Ain't an Mp3 Encoder）LAMEはMP3のエンコーダーではないの意

上に挙げたもののように言葉遊びにユーモアがありつつ皮肉がこめられているものが多くあります。

ここまでまとめた逆頭字語、再帰的頭字語と頭文字を簡単にまとめると以下のようになります。

- 再帰的頭字語 - 正式名称のなかに自身が含まれる頭文字
- 頭文字 - 英単語の頭文字をならべて作られた言葉
- 逆頭字語 - ある単語の並びから後付で頭文字としての意味をつけた言葉（頭文字の逆）

頭文字とバクロニムは対照的な関係、再帰的頭字語は名称に自身の単語が含まれるというのを抑えると覚えやすいですね。
# npmの本質とは

FAQには合わせて以下のような説明があります。

>  The precursor to npm was actually a bash utility named "pm", which was the shortform name of "pkgmakeinst" - a bash function that installed various things on various platforms. If npm were to ever have been considered an acronym, it would be as "node pm" or, potentially "new pm".

> npmの前身は、実際には "pm "という名前のbashユーティリティで、これは "pkgmakeinst "というbash関数の短縮形の名前でした。もしnpmの頭文字を取るとしたら、"node pm "か、あるいは "new pm "になるでしょう。

npmの前身となるパッケージマネージャの名前が「pm」というものだったようです。npmを頭文字をとったものとするならnpmは「node pm」「new pm」のようになると主張しています。

このことから今はNode.jsに限らす、JavaScriptのパッケージマネージャとしての立ち位置にあるnpmが**Node.jsに縛られないパッケージマネージャである**と主張しているように感じられます。このように見ていくと「npmは頭文字じゃない」を再帰的に略したものがnpmであるという説明もやや皮肉が含まれていて面白いですね。歴史を見ていく中で、npmは「Node package Manager」という意味を取り除きたいように感じます。

つまりnpmには深い意味はなく、npmはnpmであるということなのでしょう。調べつつ記事を書いていく中で頭が混乱してきたのでその結論にて終わりにします。もっと詳しく知っている方がいれば教えていただきたいです。

また、冒頭で紹介したnpmのトリックなのですが、クリックして遷移するリポジトリ[GitHub - npm/npm-expansions](https://github.com/npm/npm-expansions)ではnpmのバクロニムをプルリクエストにて募集しています。記事執筆時点で800以上のバクロニムが追加されていて驚きました。ここに登録されているものがトップページにてランダム表示されるようです。

数多くのバクロニムがありますが、私は特に以下のバクロニムが気に入りました。

> Neverending Programming Mistakes

npmの遊び心が感じられるリポジトリで寄せられたものを見ているだけでも面白いです。

# おわりに

今回はnpmのもつ意味や由来について調べてまとめてみました。npmはNode Package Managerの頭文字を並べたものだと認識していましたが、調べてみたところ全く想像もしていなかった結果となり、皮肉的な意味合いもありながらとても面白いと感じました。

調べていく中で、DVDのように時代の移り変わりで言葉のもつ持つ意味も少しづつ変わっていくのを感じました。最初は半信半疑でしたが最終的に「npmはnpmである」という結論に、なぜか納得している自分がいます。

再帰的略語や頭文字が多く飛び交うIT業界で仕事をしていると、なぜこのような名前にしたのか不思議に感じる技術を目にすることが多くあります。実際何気なく見ているものでも調べてみるとその背景には開発者の思いや、考え方が現れており、多くの発見があるように感じます。

また気になったことを調べることで得られる発見により技術がもっと好きになりますね。

長くなりましたが、最後まで読んでいただきありがとうございました。

# 参考

- [node.js - Why is npm's name not Node Package Manager? - Stack Overflow](https://stackoverflow.com/questions/45531942/why-is-npms-name-not-node-package-manager)
- [WAIT! @Myles, What DOES npm stand for?! · Discussion #639 · githubevents/universe2020 · GitHub](https://github.com/githubevents/universe2020/discussions/639)
- [頭字語 - Wikipedia](https://ja.wikipedia.org/wiki/%E9%A0%AD%E5%AD%97%E8%AA%9E)
- [バクロニム - Wikipedia](https://ja.wikipedia.org/wiki/-%E3%83%90%E3%82%AF%E3%83%AD%E3%83%8B%E3%83%A0)
- [再帰的頭字語 - Wikipedia](https://ja.wikipedia.org/wiki/%E5%86%8D%E5%B8%B0%E7%9A%84%E9%A0%AD%E5%AD%97%E8%AA%9E)
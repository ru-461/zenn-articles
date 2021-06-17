---
title: "Windows と M1 Mac の VSCode 同期戦略"
emoji: "🔄"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["vscode", "windows", "mac", "m1", "applesilicon"]
published: true
---

# はじめに

Microsoft 社が開発しているエディタ[VisualStudioCode(VSCode)](https://code.visualstudio.com/) で昨年 7 月のアップデート [version 1.48](https://code.visualstudio.com/updates/v1_48) から VSCode 設定同期機能(Settings Sync)が使えるようになり話題となりました。
VSCode には、`Stable release` と、`Inseders release` という２つのバージョンがあります。Insiders 版で先に新機能が試験的に搭載され、安定したら Stable 版にも追加されるといった形で開発が進められています。設定同期機能はもともと Insiders 版で試験的に実装されており、昨年アップデートで Stable 版でも使えるようになりました。

普段、私達がよく目にする青い VSCode は Stable 版となり、安定版と呼ばれています。Insidera 版少し緑がかった色のアイコンをしており、色で区別がつくようになっています。

**それぞれの VSCode は独立しているため、同じ PC 内に共存させることも可能です。**

![VSCodeのアイコン画像](https://storage.googleapis.com/zenn-user-upload/tgrrxh4oo27xz99wxxw1tcfb5vuh)
*左がStable版、右がInsiders版のアイコン*

昨年、VSCode 公式が Stable 版に設定同期機能を実装し話題になりましたが、今まで VSCode の設定を同期する拡張機能は存在していました。

https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync

しかし、私は当時メインマシンである Windows(WSL) で主に開発していたため、同期の拡張機能については全く触れていませんでした。昨年末に、開発用に M1 チップを搭載した Macbook Pro をお迎えしたのですが、開発環境を構築する上で小さな葛藤がありました。小さな問題ですが、設定の方法が分からず解決まで時間がかかったため、備忘録としてまとめます。

# 何が問題だったのか

VSCode の設定をしているときにしたツイートです。

https://twitter.com/Ry86163204/status/1339206380914237442?s=20

ツイート内の情報がかなり乏しく、なんのことか伝わりにくいですが、MacBook の環境構築をする際に CPU のアーキテクチャが変わったことで VSCode の環境も大きく変わりました。

:::message
2020 年２月に提供された VSCode 1.54 にて Stable 版が AppleSilicon に対応しました。
:::

## Macの新CPU アーキテクチャ問題

昨年末発売された MacBook Air、MacBook Pro、Mac mini には、従来の `Intel 製 CPU(x64)` ではなく、Apple が独自開発した新 CPU(arm64)が搭載されています。このことにより、CPU のアーキテクチャが `ARM` に置き換わったため、従来の Intel ベースのアプリを動かすために `Rosetta2` という変換ツールを介してアプリを動かすことになります。

`M1 Macに最適化` という言葉をよく聞くようになりましたが、これはアプリが `ARMアーキテクチャ` に対応し、特別な変換などをしなくともそのまま動かせるということになります。つまり M1 チップからするとネイティブな環境です。ネイティブというだけあり、パーフォーマンスやメモリの使用量などもかなり改善され、M1 チップのパワーを最大限に活用することも可能となります。

## VSCode Insidersが一足早くM1対応するも、しかし...

M1 Mac が発売してすぐの 12 月中旬 VSCode Insiders が一足早く M1 チップに対応しました。
いきなり対応したので、この情報を知ってすぐにインストールした覚えがあります。

https://twitter.com/code/status/1338886895867224070

しかし、記事執筆時点(2021 年 2 月上旬)では、VSCode(Stable 版)はまだ M1 Mac に完全対応しておらず従来のアーキテクチャで動作している状態です。
同時に起動した時のアクティビティモニタがこちらです。アーキテクチャを見ると一目瞭然ですね。
![アクティビティモニターの画像](https://storage.googleapis.com/zenn-user-upload/p583bjzskira7znwdvp72ynrz535)
*Stable版はIntel、Insiders版はAppleとなっている*

せっかく M1 チップ搭載 Mac になったので、ネイティブなアプリを使いたいという思いから私は迷わず Insiders 版をインストールしました。
~~ここで素直に Stable 版を入れれば詰まることはなかったはずですが...。~~

ここで同期がうまくできない問題に遭遇しました。Windows 版の VSCode は Stable 版を使っており、設定の同期機能をオンにしました。
続いて MacBook にインストールした Insiders 版で同期しようとしたのですが上手く、同期できませんでした。
ここでつまづき、今日に至るまで、Windows 版で使っていた拡張機能を１つずつ確認してインストールして環境を合わせながら使っておりました。正直これはかなり面倒な作業で、「Stable 版と Insiders 版で同期が上手くできれば、こんな苦労しなくていいのに...」そんな思いをしながら渋々MacBook の VSCode(Insiders)を使っていました。最初はパフォーマンス面で不安定な部分もありましたが、アップデートが積み重なり、今ではパフォーマンス面でかなり満足しています。

# 解決策

同期がうまくできないこと以外に関しては満足して今日まで使ってきました。そろそろ Stable 版も ARM アーキテクチャ対応版がでるといった話が流れ始めた頃、VSCode の公式ドキュメントを眺めていたら Stable 版と Insiders 版で設定の同期ができるとのトピックがあり、試したところ上手くいきました。

https://code.visualstudio.com/docs/editor/settings-sync#_syncing-stable-versus-insiders

## 設定同期機能をオンにする

VSCode 公式の設定同期機能はこちらからオンにできます。
この記事内では、Windows 側(Stable 版)から Mac(Insiders 版)への同期を解説します。
Stable 版同士であっても同じ手順を踏むことで同期が可能です。

:::message
Stable 版と Insiders 版で設定の同期は可能ですが、同期した際にデータの非互換性が生じる場合もある[^1]とされています。非互換性が生じた場合は自動的に同期がオフになるとのことですが、異なるビルド同士での同期は一時的なものとして使用し、環境が安定した段階で Stable 版に乗り換えて再度同期するのが無難だと思われます。
:::

[^1]: [Can I share settings between VS Code Stable and Insiders?](https://code.visualstudio.com/docs/editor/settings-sync#_can-i-share-settings-between-vs-code-stable-and-insiders)

![設定同期をオンにする様子](https://storage.googleapis.com/zenn-user-upload/nzyepfjno7qulnoaiuuffk0nj6c9)
*左下の設定アイコンクリックでメニューが現れます*

同期する設定を選択するウィンドウが現れるので選択します。デフォルトで全てオンになっています。
![同期する項目を選択する様子](https://storage.googleapis.com/zenn-user-upload/uo73sd23a1x1pxzwonx9r3cc3d53)

:::message
ショートカットキーは各プラットフォームごとに管理されるようです
:::

右上の `サインインしてオンにする` からアカウントへログインします。
サインインには `Microsoftアカウント` もしくは `GitHubアカウント` のどちらかが必要になります。
アカウント用意し VSCode の指示に従うことで簡単にサインインできます。

![使用するアカウントの選択画面](https://storage.googleapis.com/zenn-user-upload/zddtezfrdvcj9hbbfm7hj0zaqh3y)
*私はGithubアカウントでサインインしました*

右下に成功のメッセージが表示されたらうまくクラウドに保存され同期する準備ができています。
以上で同期する側の設定は終わりです。

## 設定を取り込みたいVSCodeで同期機能をオンにする

同期させたいデータをクラウドに保存できたら、同期させたい端末側での操作が必要です。
今回は、Insiders 版に Stable 版の設定を同期させます。

Mac 側に Insiders 版の VSCode をインストールし、Windows 版(Stable 版)と同じ用に設定同期機能をオンにします。
オンにする項目をチェックし選択したあと Insiders 版では、同期に使用する同期サービスを選択するウィンドウが現れます。

![同期サービスを選ぶ様子](https://storage.googleapis.com/zenn-user-upload/8h29c0led43jjwbaj57m95ggp8zo)
*Insiders版でのみ表示されます。Stable ↔ Insidersでの同期はインサイダーを選択*

これは、Stable 版と Insiders 版で**同期に使用するサービスが異なる**ために表示されるもののようです。Insiders 版が先行的に機能が実装され新しいものとなるため、データの非整合を防ぐためだそうです。

前に VSCode の設定同期をおこなったときはサービス選択画面表示されなかったような気がします。当時気づかなかっただけなのか、VSCode のアップデートで追加されたものか定かではありませんが、この画面を見たときに少し感動したのを覚えています。

それでは、Stable 版で同期をオンにしクラウドへ保存した設定を取り込みます
設定を取り込む側でも、アカウントへのサインインが求められるので、**同じアカウントを使用してサインイン**します。

これで自動的に同期が有効になりバックグラウンドで同期処理が行われます。

わかりにくいですが、VSCode の拡張機能を Windows(Stable 版)と MacBook Pro(Insider 版)で同期した様子です。Stable 版と Insiders 版ですが、うまく同期に成功しています。

![拡張機能を同期した様子](https://storage.googleapis.com/zenn-user-upload/aab05r7hljie741tucmu5rrqvmug)
*Windows(Stable 版) ・MacBook Pro(Insider 版)*

今回は起こりませんでしたが、同期を取り込む側で先に同期データが存在していた場合、設定の競合が発生します。
その場合は一旦 `設定の同期をオフ → 全てのデバイスで同期をオフにし、クラウドから同期データを消去します。` へチェックし同期機能をオフ。もう一度設定の同期機能をオンにすることで解決します。

![同期オフの確認画面](https://storage.googleapis.com/zenn-user-upload/ckks1sdedjjg25gz6spuvogalsgq)

また、設定が競合した場合に差分をマージ、手動で設定を確認しながらのマージも行えるみたいです。

設定同期機能はまだプレビュー版ではありますが不具合なく使えており満足しています。

# おわりに

今回は VSCode 公式の設定同期機能を使って Stable 版と Insiders 版を同期しました。
これで Windows マシンの環境にて使っていた VSCode 拡張機能を MacBook 側でも簡単にインストールできるようになりました。その他、同期したくない項目についても設定ファイルで柔軟に設定できるようです。つい昨年 Stable 版にて実装された機能となりまだプレビュー版ではありますが、設定がしやすくできる点、簡単に同期を行える点ではさすが公式という印象を受けました。VSCode Stable が M1 チップに対応するまでこの環境で引き続き使っていきます。

各々が使いやすいようにカスタマイズした環境をプラットフォームにとらわれることなく使えることで、VSCode の設定に悩む時間を減らし、開発に集中できますね。この記事が誰かの参考になれば幸いです。

---
最近は、VSCode の [Updates](https://code.visualstudio.com/updates/v1_53) をよく眺めているのですが、現行バージョン(1.53)で Stable 版が M１チップ(arm64)に対応する予定が、[macOS Big Sur 11.2 でWASMモジュールをロードすると拡張モジュールがクラッシュする問題](https://github.com/microsoft/vscode/issues/115646)により難航している旨の記述がありました。

M1 Mac の登場当時から多くの開発者に Stable 版でのネイティブ動作が待ち望まれてる VSCode ですが、公式のドキュメントから分かるように完全サポートされる日も近いと思われます。今後のアップデートを要チェックですね。

最後まで読んでいただきありがとうございました。

# 参考

- [Visual Studio Code January 2021](https://code.visualstudio.com/updates/v1_53#_engineering)
- [Settings Sync in Visual Studio Code](https://code.visualstudio.com/docs/editor/settings-sync#_can-i-share-settings-between-vs-code-stable-and-insiders)
---
title: "WindowsとM1 MacのVSCode同期戦略"
emoji: "🔄"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["vscode", "windows", "mac", "m1", "applesilicon"]
published: true
---

## はじめに

Microsoft社が開発しているエディタ[VisualStudioCode](https://code.visualstudio.com)の[version 1.48](https://code.visualstudio.com/updates/v1_48)にて設定同期機能（Settings Sync）が実装され話題となりました。

VSCodeには、[Stable release（Stable版）](https://code.visualstudio.com)と、[Insiders release（Insiders版）](https://code.visualstudio.com/insiders/)という２つのバージョンがあります。Insiders版で先に新機能が試験的に搭載され、安定したらStable版にも追加されるといった形で開発が進められています。設定同期機能はもともとInsiders版で試験的に実装されており、昨年アップデートでStable版でも使えるようになりました。

普段、私達がよく目にする青いVSCodeはStable releaseとなり、安定版と呼ばれています。一方Insider版は少し緑がかったアイコンをしており、色で区別がつくようになっています。

**それぞれのVSCodeは独立しているため、共存させることも可能です。**

![VSCodeのアイコン画像](/images/try-vscodesyncinsider/image01.png)
*左がStable版、右がInsiders版のアイコン*

昨年、VSCode公式がStable版に設定同期機能を実装し話題になりましたが、今までVSCodeの設定を同期する拡張機能は存在していました。
https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync

しかし、私は当時メインマシンであるWindows（WSL2）で主に開発していたため、同期の拡張機能については全く触れていませんでした。昨年末に、開発用にM1チップを搭載したMacbook Proをお迎えしたのですが、開発環境を構築する上で小さな葛藤がありました。小さな問題ですが、設定の方法が分からず解決まで時間がかかったため、備忘録としてまとめます。

# 何が問題だったのか

VSCodeの設定をしているときにしたポストです。
https://x.com/ru_461/status/1339206380914237442

ツイート内の情報がかなり乏しく、なんのことか伝わりにくいですが、MacBookの環境構築をする際にCPUのアーキテクチャが変わったことでVSCodeの環境も大きく変わりました。

:::message
2020年２月に提供されたVSCode 1.54にてStable版がAppleSiliconに対応しました。
:::

### Macの新CPU アーキテクチャ問題

昨年末発売されたMacBook Air、MacBook Pro、Mac miniには、従来のIntel製CPU（x64）ではなく、Appleが独自開発した新CPU（arm64）が搭載されています。このことにより、CPUのアーキテクチャがARMに置き換わったため、従来のIntelベースのアプリを動かすためにRosetta 2という変換ツールを介してアプリを動かすことになります。

M1 Macに最適化という言葉をよく聞くようになりましたが、これはアプリがARMアーキテクチャに対応し、特別な変換などをしなくともそのまま動かせるということになります。つまりM1チップからするとネイティブな環境です。ネイティブというだけあり、パーフォーマンスやメモリの使用量などもかなり改善され、M1チップのパワーを最大限に活用することも可能となります。

### VSCode Insidersが一足早くM1対応するも、しかし...

M1 Macが発売してすぐの12月中旬VSCode Insidersが一足早くM1チップに対応しました。
いきなり対応したので、この情報を知ってすぐにインストールした覚えがあります。
https://x.com/code/status/1338886895867224070

しかし、記事執筆時点（2021年2月上旬）では、VSCode（Stable版）はまだM1 Macに完全対応しておらず従来のアーキテクチャで動作している状態です。
同時に起動した時のアクティビティモニタがこちらです。アーキテクチャを見ると一目瞭然ですね。

![アクティビティモニターの画像](/images/try-vscodesyncinsider/image02.png)
*Stable版はIntel、Insiders版はAppleとなっている*

せっかくM1チップ搭載Macになったので、ネイティブなアプリを使いたいという思いから私は迷わずInsiders版をインストールしました。
~~ここで素直に Stable 版を入れれば詰まることはなかったはずですが...。~~

ここで同期がうまくできない問題に遭遇しました。Windows版のVSCodeはStable版を使っており、設定の同期機能をオンにしました。
続いてMacBookにインストールしたInsiders版で同期しようとしたのですが上手く、同期できませんでした。
ここでつまづき、今日に至るまで、Windows版で使っていた拡張機能を１つずつ確認してインストールして環境を合わせながら使っておりました。これはかなり面倒な作業で、「Stable版とInsiders版で同期できれば、こんな苦労しなくていいのに...」そんな思いをしながら渋々Insiders版を使っていました。最初はパフォーマンス面で不安定な部分もありましたが、アップデートが積み重なり、今ではパフォーマンス面でかなり満足しています。

## 解決策

公式ドキュメントを眺めていたらStable版とInsiders版で設定同期ができるとのトピックがあり、試したところ上手くいきました。
https://code.visualstudio.com/docs/editor/settings-sync#_syncing-stable-versus-insiders

### 設定同期機能をオンにする

VSCode公式の設定同期機能はこちらからオンにできます。
この記事内では、Windows側（Stable版）からMac（Insiders版）への同期を解説します。
Stable版同士であっても同じ手順を踏むことで同期が可能です。

:::message
Stable版とInsiders版で設定の同期は可能ですが、同期した際にデータの非互換性が生じる場合もある[^1]とされています。非互換性が生じた場合は自動的に同期がオフになるとのことですが、異なるビルド同士での同期は一時的なものとして使用し、環境が安定した段階でStable版に乗り換えて再度同期するのが無難だと思われます。
:::

[^1]: [Can I share settings between VS Code Stable and Insiders?](https://code.visualstudio.com/docs/editor/settings-sync#_can-i-share-settings-between-vs-code-stable-and-insiders)

![設定同期をオンにする様子](/images/try-vscodesyncinsider/image03.png)
*左下の設定アイコンクリックでメニューが現れます*

同期する設定を選択するウィンドウが現れるので選択します。デフォルトで全てオンになっています。
![同期する項目を選択する様子](/images/try-vscodesyncinsider/image04.png)

:::message
カスタマイズしたショートカットキーは各プラットフォームごとに管理されるようです
:::

右上の「サインインしてオンにする」からアカウントへログインします。
サインインにはMicrosoftアカウントもしくはGitHubアカウントのどちらかが必要になります。
アカウント用意しVSCodeの指示に従うことで簡単にサインインできます。

![使用するアカウントの選択画面](/images/try-vscodesyncinsider/image05.png)
*私はGitHubアカウントでサインインしました*

右下に成功のメッセージが表示されたらうまくクラウドに保存され同期する準備ができています。
以上で同期する側の設定は終わりです。

### 設定を取り込みたいVSCodeで同期機能をオンにする

同期させたいデータをクラウドに保存できたら、同期させたい端末側での操作が必要です。
今回は、Insiders版にStable版の設定を同期させます。

Mac側にInsiders版のVSCodeをインストールし、Windows版（Stable版）と同じ用に設定同期機能をオンにします。
オンにする項目をチェックし選択したあとInsiders版では、同期に使用する同期サービスを選択するウィンドウが現れます。

![同期サービスを選ぶ様子](/images/try-vscodesyncinsider/image06.png)
*Insiders版でのみ表示されます。Stable ↔ Insidersでの同期はインサイダーを選択*

これは、Stable版とInsiders版で**同期に使用するサービスが異なる**ために表示されるもののようです。Insiders版が先行的に機能が実装され新しいものとなるため、データの非整合を防ぐためだそうです。

前にVSCodeの設定同期をおこなったときはサービス選択画面表示されなかったような気がします。当時気づかなかっただけなのか、VSCodeのアップデートで追加されたものか定かではありませんが、この画面を見たときに少し感動したのを覚えています。

それでは、Stable版で同期をオンにしクラウドへ保存した設定を取り込みます
設定を取り込む側でも、アカウントへのサインインが求められるので、**同じアカウントを使用してサインイン**します。

これで自動的に同期が有効になりバックグラウンドで同期処理が行われます。

わかりにくいですが、VSCodeの拡張機能をWindows（Stable版）とMacBook Pro（Insider版）で同期した様子です。Stable版とInsiders版ですが、うまく同期に成功しています。

![拡張機能を同期した様子](/images/try-vscodesyncinsider/image07.png)
*Windows（Stable版）・MacBook Pro（Insider版）*

今回は起こりませんでしたが、同期を取り込む側で先に同期データが存在していた場合、設定の競合が発生します。
その場合は一旦設定の同期をオフ → 全てのデバイスで同期をオフにし、もう一度設定の同期機能をオンにすることで解決します。

![同期オフの確認画面](/images/try-vscodesyncinsider/image08.png)
*下のチェックにチェックをつけるとクラウド上からデータがすべて削除されます*

また、設定が競合した場合に差分をマージ、手動で設定を確認しながらのマージも行えるみたいです。

## おわりに

今回はVSCode公式の設定同期機能を使ってStable版とInsiders版を同期しました。
これでWindowsマシンの環境にて使っていたVSCode拡張機能をMacBook側でも簡単に同期できるようになりました。その他、同期したくない項目についても設定ファイルで柔軟に設定できるようです。

各々が使いやすいようにカスタマイズした環境をプラットフォームにとらわれることなく使えることで、VSCodeの設定に悩む時間を減らし、開発に集中できますね。VSCode 1.54にてStable版がAppleSiliconに対応したことで今後の開発がより捗りそうです。

この記事が誰かの参考になれば幸いです。最後まで読んでいただきありがとうございました。

## 参考

- [Visual Studio Code January 2021](https://code.visualstudio.com/updates/v1_53#_engineering)
- [Settings Sync in Visual Studio Code](https://code.visualstudio.com/docs/editor/settings-sync#_can-i-share-settings-between-vs-code-stable-and-insiders)

---
title: "Zennでの執筆体験を加速するためのエイリアス設定"
emoji: "🎢"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["zenn","zenncli"]
published: false
---

# はじめに

Zenn には [ZennCLI](https://zenn.dev/zenn/articles/install-zenn-cli) と呼ばれるツールが公式から提供されており、GitHub を利用したリポジトリ単位での記事の管理ができます。これにより、Web ブラウザ上に縛られない柔軟な執筆環境を構築できるため重宝しております。普段、記事を執筆する中で CLI のコマンドを叩くことが多くありますが、専用のコマンドを覚えてコマンドを毎回正確に叩くプロセスを簡略化して記事の執筆に専念したいと思い、エイリアスを作成することで記事の執筆に集中できる環境を作成しました。

# 作成したエイリアス

:::message
作成したエイリアスは MacOS のデフォルトシェルである Zsh で利用するものになります。また記事を管理するディレクトリへのパスは環境によって各自読み替えてください。
:::
```shell:.zshrc
alias agzenn='alias | grep zenn' # ZennCLIエイリアスの確認用

alias zenn='cd ~/Documents/my-zenn-contents' # Zennの記事ディレクトリに移動
alias zennop='cd ~/Documents/my-zenn-contents && code ~/Documents/my-zenn-contents' # Zennの記事ディレクトリに移動してVSCodeで開く
alias zennopr='cd ~/Documents/my-zenn-contents && code ~/Documents/my-zenn-contents && npx zenn preview --open'
alias zennna='npx zenn new:article' # 新しい記事をランダムなスラッグで作成
alias zennnas='npx zenn new:article --slug' # 新しい記事をスラッグを指定して作成
alias zennnb='npx zenn new:book' # 新しい本をランダムなスラッグで作成
alias zennnbs='npx zenn new:book --slug ' # 新しい本をスラッグを指定して作成
alias zennpr='npx zenn preview' # 記事と本のプレビュー
alias zennv='npx zenn --version' # ZennCLIのバージョン確認
alias zennup='sudo npm install zenn-cli@latest' # ZennCLIのアップデート
```

 # おわりに
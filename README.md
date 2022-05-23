# はじめに
Railsチュートリアルのdokcerでの環境構築方法をご紹介します。  
さらに、vscodeのRemote Container拡張機能を使用して、Railsで開発するのに必要な拡張機能がインストールされた開発環境を構築します。  
また、デバッグ実行可能です。

# Railsチュートリアルとは？
質が高い上にボリュームが非常に多く、チュートリアルをやり通すことで本格的なアプリ開発をするスタートラインに立つことができる素晴らしい教材です。  
Railsで開発をするのであれば絶対にやったほうがいいと思います。  

# 前提条件
筆者の環境がWindos11 Proなので、その前提で解説します。

## WSL2をインストール済みであること
WindowsでLinuxを利用するための仕組みです。
https://docs.microsoft.com/ja-jp/windows/wsl/install

## Docker Desktopをインストール済みであること。
https://docs.microsoft.com/ja-jp/windows/wsl/tutorials/wsl-containers

# バージョン情報
- ruby 2.7.6
- rails 6.0.4
- bundler 2.2.17

rubyのバージョンがチュートリアルとは異なります。
問題無く動作しています。

（チュートリアルでは2.6.3)

データベースはpostgresを使用しています。

# 環境構築
ホスト側に下記2つのvscode拡張機能をインストールしておきます。  
Remote - Containersを活用すると、Remoteコンテナ側でvscode拡張機能を使用することが可能になります。  
そして、Remoteコンテナ側のvscode拡張機能はRemoteコンテナのリソースを使用して動作します。  
そのため、ホスト側の環境にvscode拡張機能をインストール必要がなく、ホスト側の環境がクリーンに保たれる、というメリットがあります。  
https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers

https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl

rt-supportをgit cloneして、.gitディレクトリを削除します。  
rt-supportはRuby tutorial supportの略です。
```bash
git clone git@github.com:YamabikoHatake/rt-support.git
cd rt-support
rm -rf ./.git
```

vscodeメニューのファイル⇒フォルダーを開くからrt-supportフォルダを開きなおします。  
画面の左下をクリックし、Reopen in Containerを選択します。

![](https://storage.googleapis.com/zenn-user-upload/722f67566c6c-20220520.png)

yarn installします。
```bash
yarn install --check-files
```

データベースを作成します。
```bash
bundle exec rails db:migrate RAILS_ENV=development
```

Rails拡張機能のためのnodeモジュールが足りていないので追加します。
```bash
cd /root && yarn add compact-prefix-tree
```
追加したらRails拡張機能を有効化するために、コンテナをReopenしてください。

次に、左メニューの「実行とデバッグ」をクリックして、「Rails Server」を起動します。
デバッグ実行できるようになります。
![](https://storage.googleapis.com/zenn-user-upload/1a140002f58f-20220520.png)
Chromeを起動します。
![](https://storage.googleapis.com/zenn-user-upload/ac575237a990-20220520.png)
下記の画面が表示されたらセットアップ完了です。
![](https://storage.googleapis.com/zenn-user-upload/15c7351d489c-20220520.png)

# vscode拡張機能
リモートコンテナをオープンすると、下記の拡張機能がインストール済み、かつ、有効になっています。

## Ruby
https://marketplace.visualstudio.com/items?itemName=rebornix.Ruby
- RuboCop、Standard、およびReekによるLintサポート
- フォーマットのサポート
- シンタックスハイライトのサポート
- 基本的なインテリセンスのサポート

## Ruby Solagraph
https://marketplace.visualstudio.com/items?itemName=castwide.solargraph
Rubyのインテリセンスを強化できます。


## Ruby On Rails
https://marketplace.visualstudio.com/items?itemName=hridoy.rails-snippets
Railsのスニペットを提供してくれます。
solargraphのインテリセンスだけでは、RailsのAPI補完が不十分なので、こちらで補っています。

## Rails
https://marketplace.visualstudio.com/items?itemName=bung87.rails
Railsサポートの拡張機能です。
assetやタグのsnippetを提供しています。

## Rails DB Schema
https://marketplace.visualstudio.com/items?itemName=aki77.rails-db-schema
DB Schemaに沿って、モデルのカラム名を補完してくれたり、カラムの定義元を参照できたりします。

## ruby-rubocop
https://marketplace.visualstudio.com/items?itemName=misogi.ruby-rubocop
Ruby用のLintを実行します。

## SQL Formatter
https://marketplace.visualstudio.com/items?itemName=adpyke.vscode-sql-formatter
SQLをフォーマットできます。

## Material Icon Theme
https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme
エクスプローラー内のフォルダやファイルアイコンなどが見やすくなるテーマです。
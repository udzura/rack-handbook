## Day 1: Rackを入手

まずはじめに、[Rack](https://rubygems.org/gems/rack)をインストールしましょう。Rackは通常のRubygemとして配布されているので、インストールは[Rubygems](https://rubygems.org/)をインストールしたシェルをたちあげて以下のようにタイプするだけです。

```
gem install rack
```

Rackと言う名前には二つの側面があります。ひとつはRackの[SPEC](http://rack.rubyforge.org/doc/files/SPEC.html)という意味、もう一つは上記の手順でインストールしたRackに関する様々なユーティリティツール群をまとめたgemという意味です。

Rackは標準的なRubyの仕様にしか依存していないため、Ruby 1.8 以上のバージョンであれば問題無く動作しますし、CコンパイラのないWin32やDeveloper ToolsのないMac OS X環境でも利用が可能です。

Rack gem をインストールすることで、以下のようなものが利用可能になります。

* rackup コマンド
* 最小限の Rack アプリケーション
* 最小限の Rack ミドルウェア
* その他、基本的なユーティリティ

標準のサーバ実装であるWEBrickは1.8以降のRuby本体に標準添付されています。また、テストに関しては、 rack-test というgemが存在します。

ドキュメントに関しては、 ri と RDoc のどちらも利用可能です。以下のコマンドで明示的にインストールできます。

```
gem install rack --ri --rdoc
```

インストールの後、説明が読みたいクラスなどについて、以下のコマンドで説明が見られます。

```
ri Rack::Request
```

あるいは、以下のコマンドでRDocを閲覧するためのサーバが立ち上がります。

```
gem server
```

立ち上がったあと、 [http://localhost:8808](http://localhost:8808) にアクセスしてみてください。 


## Day 3: rackupを使う

Day 2の記事ではrackupコマンドを利用してHello World Rackアプリケーションを起動しました。

rackup はRackアプリケーションを起動するためのコマンドラインランチャーで、PSGIのplackupをインスパイアしました。`.ru`ファイルに保存されたRackアプリケーションであれば、Rackハンドラーに対応したWebサーババックエンドの上で動かすことができます。使い方はシンプルで、`.ru`ファイルのパスをコマンドに渡すだけです。

    > rackup hello.ru
    [2012-11-26 16:56:32] INFO  WEBrick 1.3.1
    [2012-11-26 16:56:32] INFO  ruby 1.9.3 (2012-11-10) [x86_64-darwin11.4.2]
    [2012-11-26 16:56:32] INFO  WEBrick::HTTPServer#start: pid=41221 port=9292

カレントディレクトリの`config.ru`という名前のファイルを起動する場合、ファイル名も省略可能です。

デフォルトで起動するバックエンドは以下の方法で選ばれます _[参考](https://github.com/rack/rack/blob/master/lib/rack/handler.rb)_ 。

* 環境特有の環境変数、たとえば `REQUEST_METHOD` や `PHP_FCGI_CHILDREN` などが定義されている場合、CGIやFCGIバックエンドが自動で選ばれます
* その他の場合、Thinが利用できる状態であればThinを、そうでない場合はWEBrickを利用します

コマンドラインスイッチ`-s`か`--server`でバックエンドを指定することもできます。

    > rackup -s puma hello.ru

rackupコマンドはデフォルトで5つのミドルウェアを有効にします。それらには `Rack::Lint`, `Rack::ShowException`, `Rack::CommonLogger`(* 他にも `Rack::Chunked`, `Rack::ContentLength` を有効にしますが、デバッグ目的としては関係がありません)が含まれ、開発の際にログやスタックトレースを表示してくれて便利ですが、これを無効にするには、`-E`または`--environment`スイッチで`development`以外の値をセットします:

    > rackup -E production -s puma hello.ru

その他のコマンドラインオプションをサーババックエンドに渡すこともでき、サーバのリッスンするポートは以下のように設定できます:

    > rackup -s puma --host 127.0.0.1 --port 8080 hello.ru
    Puma 1.6.3 starting...
    * Min threads: 0, max threads: 16
    * Environment: development
    * Listening on tcp://127.0.0.1:8080

FCGIバックエンドでUNIXドメインソケットを指定するには:

    > rackup -s fastcgi -OFile=/tmp/fcgi.sock hello.ru

その他のオプションについては、コマンドラインから`rackup --help`を実行して参照してください。明日もrackupについて解説をつづけます。

## Day 2: Hello World

プログラミング言語の学習で最初に書くプログラムは "Hello World" を表示することです。Rackでもそうしてみましょう。

**注意:** 今日のコードは理解のためにRackの生インタフェースを利用して書かれていますが、あなたがWebアプリケーションフレームワークの作者でない限り、実際にはこうしたコードを書く必要はありません。Rubyを利用したWebアプリケーションフレームワークは現在ほとんど全てのものがRackをサポートしています。例えば、Ruby Toolboxと言うサイトで[RubyのWebアプリケーションフレームワーク](https://www.ruby-toolbox.com/categories/web_app_frameworks)を人気順に見ることができます。

### Hello, World

"Hello World" を表示するための最小限のコードは以下のようになります。

    app = lambda {|env|
      [200, {'Content-Type' => 'text/plain'}, [ 'Hello World' ] ]
    }
    run app

RackアプリケーションはRubyのProcオブジェクト（ここでは`lambda`メソッドを用いて生成しています）を利用して記述が可能です。ここでは`app`という変数名に格納しています（が、名前はなんでも構いません）。このProcオブジェクトは1つのブロック引数`|env|`を受け取り、ステータス、ヘッダとボディを格納したArrayオブジェクトを返します。

このコードを`hello.ru`というファイルに保存し、rackupコマンドで起動しましょう:

    > rackup hello.ru
    [2012-11-26 16:56:32] INFO  WEBrick 1.3.1
    [2012-11-26 16:56:32] INFO  ruby 1.9.3 (2012-11-10) [x86_64-darwin11.4.2]
    [2012-11-26 16:56:32] INFO  WEBrick::HTTPServer#start: pid=41221 port=9292

rackup はアプリケーションをデフォルトのHTTPサーバ WEBrick を利用して localhost の9292番ポートで起動します。`http://127.0.0.1:9292/` をブラウザで開くと、Hello Worldが表示されましたか？

### 違うものを表示

Hello Worldはとてもシンプルなものですが、もう少し違ったことをやってみましょう。クライアント情報をRack経由で環境変数から取得し、表示します。

    app = lambda {|env|
      [
        200,
        {'Content-Type' => 'text/plain'},
        [ "Hello stranger from #{env['REMOTE_ADDR']}!" ]
      ]
    }
    run app

このコードはクライアントのリモートアドレスをRack経由の環境変数を格納したハッシュ`env`から表示します。ローカルホストで起動している場合、値は127.0.0.1 になるはずです。`env`にはHTTPの接続やクライアントに関する情報、たとえばHTTPヘッダやリクエストパスなどが格納されていて、CGIの環境変数によく似ています。

テキスト以外のデータをファイルから表示するには、以下のようにします。

    app = lambda {|env|
      case env['PATH_INFO']
      when '/favicon.ico'
        file = File.open("/path/to/favicon.ico", 'r')
        [200, {'Content-Type' => 'image/x-icon'}, file]
      when '/'
        [200, {'Content-Type' => 'text/plain'}, [ "Hello again" ]]
      else
        [404, {'Content-Type' => 'text/html'}, [ '404 Not Found' ]]
      end
    }
    run app

このアプリケーションはリクエストパスが`/favicon.ico`となっている場合に`favicon.ico`を表示し、ルート(/)については"Hello again"、その他のパスには404を返します。Rubyの標準的な`File`クラスのインスタンスはRackのボディにそのまま設定することができますし、ステータスコードについても妥当な数字をいれることができます。

<a href="https://developers.arcgis.com/javascript/latest/guide/cors/index.html" target="_blank">CORS</a> の前に、<a href="https://en.wikipedia.org/wiki/Proxy_server" target="_blank">プロキシ</a> ページで作業をする必要があります。これは同一オリジンではないリソースにアクセスするときに多くのアプリケーションが抱えていた問題を回避するために役立ちます。

CORS を有効化して使用することが推奨されますが、下記のような場合、プロキシが必要です。

- Web サーバー/ブラウザーで CORS が有効ではない、または <a href="https://en.wikipedia.org/wiki/JSONP" target="_blank">JSONP</a> 形式をサポートしていない場合
- アプリケーション内のセキュアなサービスに匿名でのアクセスが必要な時

> このトピックでは、プロキシの使用についての詳細を説明しており、CORS の使用に関する詳細は、<a href="https://developers.arcgis.com/javascript/latest/guide/cors/index.html" target="_blank">CORS</a> のガイドトピックに記載されています。

以下の例ではプロキシを使用することが適切なケースについて説明します。

### 例：サーバー上で CORS が有効ではなく、リソース レスポンスが JSONP 形式ではない場合

サーバー上で CORS が有効ではなく、<a href="https://en.wikipedia.org/wiki/JSONP" target="_blank">JSONP</a> がサポートされていないことがあります。この場合はリソースのセキュリティを回避するためにプロキシが必要とされます。

例えば、WMS サービスに対する全てのリクエストは最初に `GetCapabilities` リクエストを通過します。この最初のレスポンスは XML ファイルをロードします。もし、サーバーで CORS が有効になっていない場合、レスポンスが JSONP 形式ではないため、プロキシが必要です。

```js
/proxy.php?https://oceanwatch.pfeg.noaa.gov/thredds/wms/satellite/PP/bfp1/8day?SERVICE=WMS&REQUEST=GetCapabilities
```

### 例：サービスは JSONP をサポートしているが、サーバーで CORS が有効ではない場合

`GET` リクエストを使用する場合、プロキシは必要ありません（<a href="https://developers.arcgis.com/javascript/latest/guide/cors/index.html#example-server-without-cors-support" target="_blank">CORS</a> ガイドを参照）が、`POST` リクエストを使用するときには、プロキシが必要です。一般的に、これは以下のケースで見られます。

- アプリケーションが編集を実行する場合
- リクエストから返されたレスポンスが 2000 文字以上の場合。これは、以前強力なジオメトリ機能（バッファー処理など）を行うアプリケーションで一般的でした。最近では多くのことがクライアント側で処理されるようになっため、あまり一般的ではありません。

### 例：サービスがファイア ウォールの内側にある場合

クロスドメイン アクセスを妨げるセキュリティ設定が存在する場合があります。このような場合は、プロキシ ページを常に使用する必要があります。<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-config.html#request" target="_blank">esriConfig.request の forceProxy</a> プロパティ を使用して設定できます。 

### 例：ArcGIS プラットフォームの認証情報を持たないエンド ユーザーを対象とするアプリケーションを構築する場合

ArcGIS プラットフォームの認証情報を必要とする機能に匿名アクセスを許可するアプリケーションを作成することがあるかもしれません。いくつかのプレミアム機能には、ルート解析、バッチ ジオコーディング、および解析が含まれます。この状況では、アプリケーションはプロキシに格納されている認証情報を使用してプラットフォームへログインします。詳細については <a href="https://developers.arcgis.com/authentication/#app-login" target="_blank">ArcGIS Security and Authentication</a> を参照してください。

---

次のステップでは、プロキシを使用する手順を説明します。

## プロキシを取得する

プロキシは、Web サーバーが ArcGIS Server インスタンスをホストしている場合を除いて、Esri サーバーまたは ArcGIS Server がインストールされているコンピューターではなく、ローカル Web サーバーで実行されます。<a href="https://github.com/Esri/resource-proxy/releases" target="_blank">3 つのプロキシをダウンロード</a>でき、各プロキシは特定のサーバー サイドのプラットフォームを対象としています。

- <a href="https://github.com/Esri/resource-proxy/tree/master/DotNet" target="_blank">ASP.NET</a>
- <a href="https://github.com/Esri/resource-proxy/tree/master/Java" target="_blank">Java/JSP</a>
- <a href="https://github.com/Esri/resource-proxy/tree/master/PHP" target="_blank">PHP</a>

お使いのプラットフォームに適したプロキシを<a href="https://github.com/Esri/resource-proxy/releases" target="_blank">ダウンロード</a>してください。ダウンロードしたプロキシにはインストール手順とシステム要件の情報が含まれており、手順に従って Web サーバーのプロキシをセットアップして構成してください。

## プロキシを使用する

アプリケーションでプロキシを介してリクエストをするためには、プロキシがホストされている場所を定義するコードをアプリケーションに追加する必要があります。

- アプリケーション内のすべてのリクエストが同じプロキシを使用する場合、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-config.html#request" target="_blank">request オブジェクトの `proxyUrl` プロパティ</a> を使用してプロキシの場所を指定します。<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-config.html#request" target="_blank">request オブジェクトの `forceProxy` プロパティ</a> を使用して、リクエストが常にプロキシを通すかどうかを指定することもできます。これによりアプリケーションは、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-config.html#request" target="_blank">request</a> を使用して行われたすべての AJAX リクエストと `<img>` 要素を介して追加された画像に対してプロキシを使用するよう強制されます。

```js
require(["esri/config"], function(esriConfig) {
  esriConfig.request.proxyUrl = "/resource-proxy/Java/proxy.jsp";
  esriConfig.request.forceProxy = true;
});
```

- 特定のプロキシ ルールを使用してアプリケーションを構成することもできます。これらのルールは、同一の URL プレフィックスを持つ特定のリソースのプロキシを定義します。アプリケーションがこの URLを介してリソースにアクセスしようとすると、リクエストは指定されたプロキシを介して送信されます。<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-config.html#request" target="_blank">request の `proxyRules` プロパティ</a>はこれらの全てのプロキシ ルールをリストにしたオブジェクトです。これを設定するには、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-core-urlUtils.html#addProxyRule" target="_blank">urlUtils.addProxyRule()</a> メソッドを使用します。

```js
require(["esri/core/urlUtils"], function(urlUtils) {
  urlUtils.addProxyRule({
    urlPrefix: "route.arcgis.com",
    proxyUrl: "/resource-proxy/Java/proxy.php"
  });
});
```

## アプリケーションのテストとデプロイ

アプリケーションのプロキシ ページを構成したら、リクエストが正しく処理されているかアプリケーションをテストします。アプリケーションは、プロキシ ページが実装される前と同じように正常に機能するはずです。そうでない場合は、プロキシのトラブルシューティングが必要な場合があります。

- アプリケーション環境でデバッグ モードがサポートされている場合は、プロキシ ページにブレークポイントを設定して、正しく動作しているかどうかを確認することができます。例えば、IIS/ASP.NET 環境では、Visual Studio でアプリケーションを開き、proxy.ashx の `ProcessRequest` メソッドにブレークポイントを設定し、アプリケーションをデバッグ モードで実行できます。実行はブレークポイントで停止するはずで、どこに問題があるかを検出することができます。また、アプリケーションの JavaScript 関数でブレークポイントを設定することも、実行中に値を表示するためにコンソール ステートメントを挿入することもできます。

- 各プロキシは関連する `config` ファイルを持っています。すべてのリクエストをプロキシを介して行うには、`config` ファイルの `mustMatch` 属性を _false_ に設定します。アプリケーションが現在動作している場合、`serverUrls` セクションにサービスがリストされていないか、サーバー URL に誤字がある可能性があります。プロキシのトラブルシューティングが完了したら、`mustMatch` 属性を _true_ に戻してください。

- プロキシのログを有効にします。有効にすると、問題のトラブルシューティングに役立つメッセージがログに書き込まれます。

- アプリケーション コードでプロキシの正しい場所を指定しているかどうかを確認してください。ブラウザーの開発者ツールを使用して、プロキシが正常に配置されているかどうかを判断できます。これを行うには、ブラウザーのデバッグ ツールを有効にして、ネットワーク リクエストを調べます。特に、プロキシへの `POST` リクエストに注目します。404 エラーが表示された場合、これはプロキシが見つからなかったことを意味します。リクエストのプロパティを調べて、アプリケーションがプロキシを探しているパスを表示します。

## プロキシ アプリケーションのセキュリティ

アプリケーションがトークンベースのセキュリティでサービスを使用していて、プロキシが `username` と `password` または `client_id` と`client_secret` で構成されている場合、認証されたアプリケーションだけがアクセスできるようにプロキシ アプリケーションを保護する必要があります。詳細については、 <a href="https://developers.arcgis.com/authentication/" target="_blank">ArcGIS Security and Authentication</a> のドキュメントをご参照ください。

## 関連リンク

- <a href="https://github.com/Esri/resource-proxy/releases" target="_blank">Esri-provided resource proxies</a>
- <a href="https://developers.arcgis.com/authentication/working-with-proxies/" target="_blank">Working with proxy services</a>
- <a href="https://developers.arcgis.com/authentication/working-with-proxies/#self-hosted-proxy-service" target="_blank">Working with self-hosted proxies</a>
- <a href="https://developers.arcgis.com/javascript/latest/sample-code/tasks-route/index.html" target="_blank">Sample - Routing sample</a>
- <a href="https://developers.arcgis.com/javascript/latest/sample-code/elevation-query/index.html" target="_blank">Sample - Query Elevation (lines)</a>

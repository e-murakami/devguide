ArcGIS API for JavaScript は <a href="https://en.wikipedia.org/wiki/Cross-origin_resource_sharing" target="_blank">CORS</a> をサポートしています。CORS は、Web アプリケーションにブラウザーの<a href="https://en.wikipedia.org/wiki/Same-origin_policy" target="_blank"> 同一オリジン ポリシー</a> をバイパスし、他のサーバー / ドメイン上のリソースまたはサービスにアクセスできます。

Web サーバーとブラウザーの両方が CORS をサポートしている場合、<a href="https://en.wikipedia.org/wiki/Proxy_server" target="_blank">プロキシ</a>はクロスドメイン要求を行う必要はありません。これは次のように役立ちます。

- サーバーが目的のリソースにアクセスするのを待ってからクライアントに返す前に結果を受け取り、Web アプリケーションが要求をサーバーに送り返す必要がなくなるため、パフォーマンスが向上します。
- サーバー上にプロキシ ページを維持する必要がなくなり、開発が容易になります。

> このトピックでは CORS について詳しく説明していますが、プロキシの操作に関する情報は <a href="https://developers.arcgis.com/javascript/latest/guide/proxies/index.html" target="_blank">プロキシガイド</a> のトピックにあります。

> CORS をサポートするために Web サーバーを事前に構成する必要がありますが、ブラウザーも CORS をサポートできる必要があります。Web サーバーで CORS を有効にする方法については、<a href="https://enable-cors.org/" target="_blank">enable-cors.org</a> をご覧ください。最新のブラウザーはすべて<a href="http://caniuse.com/#feat=cors" target="_blank"> CORS をサポート</a> しています。

## ArcGIS API for JavaScript と CORS

ArcGIS API for JavaScript は、CORS サポートの自動検出機能を備えています。サービスをロードするとき、API アプリケーションで使用されているサーバーの `/rest/info` エンドポイントに `XHR` リクエストを送信します。

![stamenCorsHeader.png](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/stamenCorsHeader.png)

上の画像は、`/rest/info` エンドポイントとそれが返す応答ヘッダーを示しています。`Access-Control-Allow-Origin` ヘッダーに注意してください。これは、サーバーが CORS に対応していて、標準の `XMLHttpRequest` が同一ドメイン上にあるかのようにレスポンスにアクセスすることができます。

### 例：CORS サポートなしのサーバー

下の画像は、`https://sampleserver1.arcgisonline.com/arcgis/rest/info?=json` へのリクエストが行われています。

![SS1NoCorsHeader.png](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/SS1NoCorsHeader.png)

Sampleserver1 は CORS が有効になっていないため、この要求は失敗し、クロス ドメイン要求を許可できないことを示すエラーがブラウザーの開発者コンソールに記録されます。

> <span style="color:red">XMLHttpRequest cannot load {REQUESTED-URL}. No 'Access-Control-Allow-Origin' header on the requested resource. Origin '<APP-DOMAIN>' is therefore not allowed access.</span>

Sampleserver1 はバージョン 10.01 のArcGIS Server サービスです。バージョン 10.1 より前のバージョンでは、ArcGIS Server サービスは CORS が有効になっていませんでした。この警告は、10.1以降のバージョンで実行されているサービスでは表示されず、前のバージョンで実行されていたサービスに表示されても無視されます。

`sampleserver1.arcgisonline.com/rest/services` で、CORS が有効になっていない場合でも、このサーバーのレイヤーは引き続き表示できます。「ドメイン間アクセスが有効になっていないと、どのようにクロス ドメインにアクセスするのか？」と思うかもしれません。

これは、リクエストが `HTTP GET` を介して行われ、レスポンスが JSONP（JSON with padding）形式であるためです。これは URL で指定されたコールバック関数でラップされた JSON レスポンスです。例えば、コールバックを使用します。

```js
https://sampleserver1.arcgisonline.com/ArcGIS/rest/services/Demographics/ESRI_Population_World/MapServer?f=json&dpi=96&transparent=true&format=jpeg&callback=dojo.io.script.jsonp_dojoIoScript1._jsonpCallback
```

下のコードのようなレスポンスに注目してください。

```js
dojo.io.script.jsonp_dojoIoScript1._jsonpCallback({
    "currentVersion": 10.01,
    "serviceDescription": "This service contains population density polygons, country boundaries, and city locations for the world. The map is color coded based on the number of persons per square mile (per every 1.609 kilometers square). Population data sources included national population censuses, the United Nations demographic yearbooks, and others. In general, data currency ranged from 1981 to 1994. This is a sample service hosted by ESRI, powered by ArcGIS Server. ESRI has provided this example so that you may practice using ArcGIS APIs for JavaScript, Flex, and Silverlight. ESRI reserves the right to change or remove this service at any time and without notice.",
    "mapName": "Layers",
    "description": "This service contains population density polygons, country boundaries, and city locations for the world. The map is color coded based on the number of persons per square mile (per every 1.609 kilometers square). Population data sources included national population censuses, the United Nations demographic yearbooks, and others. In general, data currency ranged from 1981 to 1994.\n",
    "copyrightText": "(c) ESRI and its data partners",
    "layers": [{
        "id": 0,
        "name": "CEISEN Population",
        "parentLayerId": -1,
        "defaultVisibility": true,
        "subLayerIds": null,
        "minScale": 0,
        "maxScale": 0
    }],
    "tables": [],
    "spatialReference": {
        "wkid": 4326
    },
    "singleFusedMapCache": false,
    "initialExtent": {
        "xmin": -781.528595186209,
        "ymin": -399.851519127974,
        "xmax": 778.578125335329,
        "ymax": 423.455277935017,
        "spatialReference": {
            "wkid": 4326
        }
    },
    "fullExtent": {
        "xmin": -180,
        "ymin": -90,
        "xmax": 180,
        "ymax": 90,
        "spatialReference": {
            "wkid": 4326
        }
    },
    "units": "esriDecimalDegrees",
    "supportedImageFormatTypes": "PNG24,PNG,JPG,DIB,TIFF,EMF,PS,PDF,GIF,SVG,SVGZ,AI,BMP",
    "documentInfo": {
        "Title": "GlobalPopulation",
        "Author": "serverxadmin",
        "Comments": "",
        "Subject": "",
        "Category": "",
        "Keywords": "",
        "Credits": ""
    },
    "capabilities": "Map,Query,Data"
});
```

このアプローチを使用するには、API が `HTTP GET` エンドポイントを要求し、標準の JSON データの代わりに JavaScript コードを返す必要があります。アプリケーションは、JSON データを含む Web ページに `<script>` タグを作成して挿入し、関数呼び出しをラップします。これは、タグの `src` 属性で設定されます。これにより、クロスドメインのセキュリティ上の問題が回避され、サービスへのアクセスが許可されます。

>エラーは無害のため、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-config.html#request" target="_blank">`esriConfig.request's corsDection` プロパティ</a> を false に設定することで抑制できます。

### 例：API が `/rest/info` エンドポイントにリクエストを送信しない

API が `/rest/info` エンドポイントにリクエストを送信しない場合がいくつかあります。

- API にはCORS をサポートするサーバー名のリストが含まれています。サーバーがこの <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-config.html#request" target="_blank">`esriConfig.request's corsDection` プロパティ</a> にすでにリストされている場合、それらの サーバー `/rest/info` エンドポイントへのリクエストは送信されません。
- <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-config.html#request" target="_blank">`esriConfig.request's corsDection` プロパティ</a> が false の場合
- ブラウザーが CORS をサポートしていない場合（一般的ではない）

### 例：CORS がサポートされている

サーバーで CORS が有効になっていることを、このリソースにアクセスしているアプリケーションが認識していない場合があります。この状況では以下のような開発者コンソール エラーが表示されることがあります。

> <span style="color:red">[esri.core.urlUtils] esri/config: esriConfig.request.proxyUrl is not set. If making a request to a CORS-enabled server, please push the domain into esriConfig.request.corsEnabledServers.</span>

これを回避するには CORS 対応サーバーのルート URL の文字列の配列を <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-config.html#request" target="_blank">`esri.config.defaults.io.corsEnabledServers`</a> に追加します。

```js
// 既知のサーバーをリストに追加します。
require(["esri/config"], function(esriConfig) {
  esriConfig.request.corsEnabledServers.push("www.example.com");
});
```

これは、ArcGIS Server および <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-WebTileLayer.html" target="_blank">Web タイル レイヤー</a>に使用されるサービスなどサード パーティ サービスに使用できます。

デフォルトでは、API は自動的にいくつかのサーバーを有効にします。例えば、 `server.arcgisonline.com` は CORS でサポートされているサーバーとして自動的に認識される `corsEnabledServer` です。自動的に有効になっている全てのサーバーのリストは、API リファレンスの <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-config.html#request" target="_blank">`esri.config.defaults.io.corsEnabledServers`</a> をご覧ください。

><a href="http://test-cors.org/" target="_blank">test-cors.org</a> はアクセスするサーバーが CORS をサポートしているかどうかわからない場合に役立つリソースです。

### その他のシナリオ

上記の使用例に合わない場合があります。例えば、CORS がサーバー上で有効になっておらず、JSONP がサポートされていない場合や、サービスがファイアウォールの内側にある場合があります。このような場合は、<a href="https://en.wikipedia.org/wiki/Proxy_server" target="_blank">プロキシ</a> ページが必要です。プロキシ ページに関する使用事例については、<a href="https://developers.arcgis.com/javascript/latest/guide/proxies/index.html" target="_blank">proxies</a> ガイドをご参照ください。

### サンプル

以下のサンプルは <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-config.html#request" target="_blank">esri.config.defaults.io.corsEnabledServers</a>
 にサーバー名をプッシュする方法を示しています。

- <a href="https://developers.arcgis.com/javascript/latest/sample-code/basemap-custom/index.html" target="_blank">Custom basemap</a>
- <a href="https://developers.arcgis.com/javascript/latest/sample-code/layers-csv/index.html" target="_blank">CSV Layer</a>
- <a href="https://developers.arcgis.com/javascript/latest/sample-code/layers-webtile-3d/index.html" target="_blank">Web Tile Layer</a>

## 追加情報
- <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-config.html#request" target="_blank">API Reference: esri/config</a>
- <a href="https://enable-cors.org/" target="_blank">Enable web server for CORS support</a>
- <a href="http://test-cors.org/" target="_blank">Test whether server supports CORS</a>
- <a href="https://codepen.io/iospadov/post/when-cors-got-your-json-down-using-jsonp-to-avoid-blocking-of-cross-origin-requests" target="_blank">CodePen's When CORS got your JSON down article</a>
- <a href="https://www.getfilecloud.com/blog/using-jsonp-for-cross-domain-requests/" target="_blank">FileCloud's Using JSONP for cross domain requests</a>
- <a href="https://developers.arcgis.com/javascript/latest/guide/proxies/index.html" target="_blank">Proxy page guide topic</a>

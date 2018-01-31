オートキャストは、`require()` 関数を介して余分な API モジュールを参照することなく、型付けされたプロパティを設定する便利な方法です。開発者はコンストラクター パラメーターを目的のプロパティに直接渡すことができます。API は、提供されたコンストラクター パラメーターを使用して、内部的にオブジェクトをインスタンス化し、プロセスをより簡単にします。

レンダラーを作成する一般的なコードを見てみましょう。ポイント データを含む <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html" target="_blank">フィーチャ レイヤー</a> の <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-SimpleRenderer.html" target="_blank">シンプル レンダラー</a> を作成するには、5つのモジュールが必要です。


```js
require([
  "esri/Color",
  "esri/symbols/SimpleLineSymbol",
  "esri/symbols/SimpleMarkerSymbol",
  "esri/renderers/SimpleRenderer",
  "esri/layers/FeatureLayer",
], function (
  Color, SimpleLineSymbol, SimpleMarkerSymbol, SimpleRenderer, FeatureLayer
) {
  var diamondSymbol = new SimpleMarkerSymbol({
    style: "diamond",
    color: new Color([255, 128, 45]),  // 通常は "new" キーワードを使用してインスタンスを作成します
    outline: new SimpleLineSymbol({    // 通常は "new" キーワードを使用してインスタンスを作成します
      style: "dash-dot",
      color: new Color([255, 128, 45]) // 通常は "new" キーワードを使用してインスタンスを作成します
    })
  });
  var layer = new FeatureLayer({
    url: "https://services.arcgis.com/V6ZHFr6zdgNZuVG0/arcgis/rest/services/WorldCities/FeatureServer/0",
    renderer: new SimpleRenderer({
      symbol: diamondSymbol
    })
  });
});
```

ドキュメントを見ると、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-SimpleMarkerSymbol.html" target="_blank">SimpleMarkerSymbol</a> の <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-SimpleMarkerSymbol.html#color" target="_blank">color</a> と <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-SimpleMarkerSymbol.html#outline" target="_blank">outline</a> のプロパティをオートキャストできることがわかります。フィーチャ レイヤーの <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html#renderer" target="_blank">renderer</a> プロパティと<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-SimpleRenderer.html" target="_blank">シンプル レンダラー</a>の <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-SimpleRenderer.html#symbol" target="_blank">symbol</a> プロパティに同じ `autocast` タグがあります。

オートキャストを使ってみましょう。モジュールを読み込む必要はありません。コンストラクター パラメーターを直接 autocast プロパティに渡すだけです。

```js
require([
  "esri/layers/FeatureLayer"
], function (
  FeatureLayer
) {
  var diamondSymbol = {
    type: "simple-marker",  // new SimpleMarkerSymbol() としてオートキャスト
    style: "diamond",
    color: [ 255, 128, 45 ],  // new Color() としてオートキャスト
    outline: {              // SimpleLineSymbol() としてオートキャスト
      style: "dash-dot",
      color: [ 255, 128, 45 ] // ここでも、new Color() を指定する必要はありません
    }
  };
  var layer = new FeatureLayer({
    url: "https://services.arcgis.com/V6ZHFr6zdgNZuVG0/arcgis/rest/services/WorldCities/FeatureServer/0",
    renderer: {
      type: "simple",  // new SimpleRenderer() としてオートキャスト
      symbol: diamondSymbol
    }
  });
});
```

このスニペットが読み込んでいるモジュールは、前出のスニペットより 4 つ少なく、コードはより簡単ですが、機能的には同じです。API は、これらのプロパティで渡した値を受け取り、型付けされたオブジェクトを内部的にインスタンス化します。

モジュールのタイプがわかっている、あるいは固定されているプロパティでは、`type` を指定する必要はありません。例えば、上のスニペットの <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-SimpleMarkerSymbol.html#outline" target="_blank">SimpleMarkerSymbol</a> の `outline` プロパティを見てください。`type` が指定されていません。`SimpleLineSymbol` のインスタンスのみが `outline` プロパティに適用されるため、`type` プロパティを含む必要はありません。

```js
var diamondSymbol = {
    type: "simple-marker",
    outline: {
      type: "simple-line",  // `simple-line` がすでに定義されているため、このタイプを指定する必要はありません
      style: "dash-dot",
      color: [ 255, 128, 45 ]
    }
  };
```

<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html#renderer" target="_blank">FeatureLayer.renderer</a> のように、より一般的な `type ` の場合は、常に `type` を指定する必要があります。

```js
var layer = new FeatureLayer({
  renderer: {
      // シンプル、数値分類、個別値分類のうち、どの種類のレンダラーが使用されるのでしょうか？
      // `type` を指定せずに API へどの種類を使用するのか伝える方法はありません
    defaultSymbol: diamondSymbol
  }
});
```

## サポートされているプロパティ

API ドキュメントで次のラベルを持つプロパティの値は、オートキャストに対応しています。

![autocast-label](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/Autocasting/autocast-label.png)


## その他の例

型付けされたプロパティをオートキャストして<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-Viewpoint.html" target="_blank">ビューポイント</a> を作成してみましょう。ビューポイントの <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-Viewpoint.html#camera" target="_blank">カメラ</a> プロパティ、カメラの <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-Camera.html#position" target="_blank">ポジション</a> プロパティ、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Point.html" target="_blank">ポイント</a> の <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Point.html#spatialReference" target="_blank">空間参照</a>プロパティはすべてオートキャストによって設定できます。

```js
require([
  "esri/Viewpoint"
], function(Viewpoint){
  // 必要なモジュールは Viewpoint のみです
  var viewpt = new Viewpoint({
    camera: {  // new Camera() としてオートキャスト
      heading: 90,
      tilt: 30,
      position: {  // new Point() としてオートキャスト
        x: -9177811,
        y: 4247784,
        z: 50000,
        spatialReference: { wkid: 3857 }  // new SpatialReference() としてオートキャスト
      }
    }
  });
});
```

オートキャストは、開発者の利便性を高め、API の使用を簡素化するように設計されています。

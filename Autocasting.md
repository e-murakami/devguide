オートキャストは、`require()` 関数を介して余分な API モジュールを参照することなく型付きプロパティを設定する便利な方法です。開発者はコンストラクター パラメーターを目的のプロパティに直接渡すことができます。API は内部的に提供されたコンストラクター パラメーターを使用してオブジェクトをインスタンス化し、プロセスをより簡単にします。

レンダラーを作成する一般的なコードを見てみましょう。ポイント データを含む <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html" target="_blank">フィーチャ レイヤー</a> の <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-SimpleRenderer.html" target="_blank">SimpleRenderer</a> を作成するには、5つのモジュールが必要です。


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
    color: new Color([255, 128, 45]),  // 通常は “new” キーワードを使用してインスタンスを作成します。
    outline: new SimpleLineSymbol({    // 通常は “new” キーワードを使用してインスタンスを作成します。
      style: "dash-dot",
      color: new Color([255, 128, 45]) // 通常は “new” キーワードを使用してインスタンスを作成します。
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

ドキュメントを見ると、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-SimpleMarkerSymbol.html" target="_blank">SimpleMarkerSymbol</a>
 の <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-SimpleMarkerSymbol.html#color" target="_blank"> color </a>
 と <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-SimpleMarkerSymbol.html#outline" target="_blank"> outline </a>
 のプロパティをオートキャストすることができることがわかります。フィーチャ レイヤーの <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html#renderer" target="_blank"> renderer </a>
 プロパティと <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-SimpleRenderer.html" target="_blank">SimpleRenderer</a>
 の <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-SimpleRenderer.html#symbol" target="_blank">symbol</a> プロパティに同じ `autocast` タグがあります。

次は、オートキャスティングを取り入れます。コンストラクター パラメーターを直接 autocast プロパティに渡すだけです。

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
      color: [ 255, 128, 45 ] // ここでも、new Color() を指定する必要はありません。
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

このスニペットのモジュールは前出のスニペットより 4 つ少なく、コードはより簡単ですが、機能的に同じであることに注目してください。API はこれらのプロパティで渡した値を受け取り、型付きオブジェクトを内部的にインスタンス化します。

モジュールのタイプがわかっている、あるいは固定されているプロパティでは、`type` を指定する必要はありません。例えば、上のスニペットの <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-SimpleMarkerSymbol.html#outline" target="_blank">SimpleMarkerSymbol</a>
 の `outline` プロパティを見てください。 `type` が指定されていないことがわかります。`SimpleLineSymbol` のインスタンスのみが `outline` プロパティに適用されるため、`type` プロパティを含む必要はありません。

```js
var diamondSymbol = {
    type: "simple-marker",
    outline: {
      type: "simple-line",  // `simple-line` がすでに定義されているため、このタイプを指定する必要はありません。
      style: "dash-dot",
      color: [ 255, 128, 45 ]
    }
  };
```

<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html#renderer" target="_blank">FeatureLayer.renderer</a> のように、よりジェネリックな `type ` の場合は、オートキャスティングを有効にするには常に `type` を指定する必要があります。

```js
var layer = new FeatureLayer({
  renderer: {
      // レンダラーの種類を指定します。（Simple/Class breaks/Unique value）
      // `type` を指定せずに API が何をすべきか指示を出す方法はありません。
    defaultSymbol: diamondSymbol
  }
});
```

## サポートされているプロパティ

API ドキュメントで次のラベルを持つプロパティの値は、オートキャストに対応しています

![autocast-label](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/Autocasting/autocast-label.png)


## その他の例

型付けされたプロパティをオートキャストしてビューポイントを作成します。<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-Viewpoint.html" target="_blank">ビューポイント</a> の <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-Viewpoint.html#camera" target="_blank">camera</a> プロパティ、カメラの <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-Camera.html#position" target="_blank">position</a> プロパティ、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Point.html" target="_blank">ポイント</a> の <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Point.html#spatialReference" target="_blank">spatialReference</a> プロパティはすべてオートキャストによって設定できます。

```js
require([
  "esri/Viewpoint"
], function(Viewpoint){
  // Viewpoint は必要とされる唯一のモジュールです。
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




## promise の操作

promise は ArcGIS API for JavaScript の 4.x では重要な役割を果たします。promise を理解すればするほどアプリケーションで非同期操作を行う際に、より洗練されたコードを書くことができるようになります。

## promise とは何か

promise は非同期タスクから返される将来の値の代理です。タスクが実行されると、promise は他のプロセスを同時に実行させるため、アプリケーションのパフォーマンスが最適化し、ユーザーによりよい体験が提供されます。promise はプロセスが完了するたびに返されることを「約束」する値です。これは、タイミングとダウンロード速度が予測できない複数のネットワーク リクエストを行う場合に特に便利です。

promise は、pending、resolved、rejected の 3 つのステータスのいずれかの状態にあります。promise が解決したとき（resolved）は、`callback` 関数で定義されている別の promise や値を解決することができます。promise が拒否されたとき（rejected）は、`errback` 関数で処理する必要があります。

## .then() を理解する

promise は一般的に `.then()` メソッドと一緒に使用されます。`.then()` メソッドは、`callback` 関数と `errback` 関数を定義する強力なメソッドです。最初のパラメータは常に `callback` で、2 番目のパラメータは `errback` です。

```js
someAsyncFunction.then(callback, errback);
```

promise が解決されると `callback` が呼び出され、promise が拒否されると `errback` が呼び出されます。

```js
someAsyncFunction().then(function(resolvedVal){
  // これは promise が解決したときに呼び出されます。
  console.log(resolvedVal);  // promise が解決する値をコンソールに表示します。
}, function(error){
  // この関数は promise が拒否されたときに呼び出されます。
  console.error(error);  // エラーメッセージを表示します。
});
```

`otherwise` 関数を使用して promise が拒否されたときのエラーを処理することもできます。詳細は <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-core-Error.html" target="_blank">Esri Error</a> をご覧ください。

```js
button.on("click", function() {
  esriRequest(url, options).then(function(response) {
    //ここにコードを書きます。
  }).otherwise(function(error){
    console.log("informative error message: ", error);
  });
});
```

### 例：GeometryService.project()

この例では、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-tasks-GeometryService.html" target="_blank">ジオメトリ サービス</a> を使用して、いくつかのポイント ジオメトリを新しい空間参照に投影します。<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-tasks-GeometryService.html#project" target="_blank">GeometryService.project()</a> のドキュメント を見ると、project() は投影されたジオメトリの配列を解決する promise を返すことがわかります。

```js
require([
  "esri/tasks/GeometryService",
  "esri/tasks/support/ProjectParameters", ...
  ], function(GeometryService, ProjectParameters, ... ) {
    // ジオメトリ サービスのインスタンスを作成します。
    var gs = new GeometryService( "http://sampleserver6.arcgisonline.com/arcgis/rest/services/Utilities/Geometry/GeometryServer" );
    // 投影パラメータを設定します。
    var params = new ProjectParameters({
      geometries: [points],
      outSR: outSR,
      transformation = transformation
    });
    // project 関数を実行します。
    gs.project(params).then(function(projectedGeoms){
      // promise は projectedGeoms に格納された値を解決します。
      console.log("projected points: ", projectedGeoms);
      // .then() 関数の内部にある投影されたジオメトリを持つものを実行します。
    }, function(error){
      //promise が拒否されたときはエラーを返す
      console.error(error);
    });
});
```

## promise の連鎖

promise を使用する利点の一つは、 `.then()` を活用して複数の promise を連鎖させることです。これにより、ネストされたコールバックの後にあるネストされたコールバックを削除することで、コードを綺麗に整理できます。複数の promise を連鎖させる場合、.`then()` によって定義された各 promise の値は、前回の promise の解決に依存します。これにより、複数のコールバックを互いにネストすることなく、コード ブロックを連続させることができます。別の `.then()` に値を提供するそれぞれの `.then()` が、 `return` コマンドを使用しなければならないことに注意が必要です。


### 例：geometryEngineAsync.geodesicBuffer()

上のコードの続きで、複数の `.then()` 関数を使用して分析を行うサンプルです。他の promise の連鎖の例は、<a href="https://developers.arcgis.com/javascript/latest/sample-code/chaining-promises/index.html" target="_blank">Chaining Promises sample</a> をご覧ください。

```js
/***************************************
  * 上のコードの続きです。
  ****************************************/

  // バッファー グラフィックスを格納するための GraphicsLayerインスタンスを作成します。
  var bufferLayer = new GraphicsLayer();
  // ポイントを投影し、他の関数に promise をつなげます。
  gs.project(params)
    .then(bufferPoints)             // project() が解決されたとき、ポイントは bufferPoints() に送られます。
    .then(addGraphicsToBufferLayer) // bufferPoints() が解決されたとき，バッファーはaddGraphicsToBufferLayer() に送られます。
    .then(calculateAreas)           // addGraphicsToBufferLayer() が解決されたとき,バッファーは calculateAreas() に送られます。
    .then(sumArea)
    .otherwise(function(error){ // calculateAreas() が解決されたとき, エリア は sumArea()に送られます。
      console.error('One of the promises in the chain was rejected! Message: ', error);
    });

  // 各関数が連鎖内の次の関数で使用される値を返す方法に注目してください。

  // 配列の各ポイントから1000フィートのバッファーを作成します。
  function bufferPoints(points) {
    return geometryEngine.geodesicBuffer(points, 1000, 'feet');
  }

  // 各バッファの新しいグラフィックを作成し、bufferLayer に追加します。
Creates a new graphic for each buffer and adds it to the bufferLayer
  function addGraphicsToBufferLayer(buffers) {
    buffers.forEach(function(buffer) {
      bufferLayer.add(new Graphic(buffer));
    });
    return buffers;
  }

  // 各バッファーの面積を計算し、新しい配列に追加します。
  function calculateAreas(buffers) {
    return buffers.map(function(buffer) {
      return geometryEngine.geodesicArea(buffer, 'square-feet');
    });
  }

  // 以前の .then（） から返された配列内の全ての合計を計算します。
  function sumArea(areas) {
    var total = 0;
    areas.forEach(function(area) {
      total += area;
    });
    console.log("Total buffered area = ", total, " square feet.");
  }
```

## promise としての API クラス

ArcGIS API for JavaScript の多くのメソッドは promise を返します。例えば、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-tasks-Task.html" target="_blank">task </a>モジュール と <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-geometryEngineAsync.html" target="_blank">GeometryEngineAsync</a> モジュールの全てのメソッドは promise を返します。

関数は promise を返すだけではなく、API のいくつかのクラスは promise として非同期で初期化されます。例えば、`MapView` と `SceneView` は有効な ` container` 値と `map` を持つなど、いくつかの条件が満たされたときに準備が整います。これらのクラスは、一度完了されると promise オブジェクトのように動作しますが、わずかに異なる API を持っています。ネイティブの promise のようにクラスの `then()` を呼び出すのではなく、インスタンスが完全に初期化されたときに通知されるクラス インスタンスに直接 `when()` を呼び出します。以下のクラスはそれらのインスタンスを作成すると promise になります。 

-	<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-Layer.html" target="_blank">All layers</a>
-	<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html" target="_blank">MapView</a>
-	<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html" target="_blank">SceneView</a>
-	<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-ViewAnimation.html" target="_blank">ViewAnimation</a>
-	<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-layers-LayerView.html" target="_blank">LayerView</a>

> promise vs イベント リスナー<br>
> `when()` メソッドは、イベント リスナーと同様の動作をするように見えます。しかし、promise が完了後に非同期プロセスの結果に直接アクセスできるのに対して、イベントが発生した後にイベント リスナーを初期化するとリスナーは決して起動しません。

### 例：MapView

この例では、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html" target="_blank">MapView</a> のインスタンスで `when()`を使用します。`view.when()`のコールバックのコードはビューの準備ができるまで実行されません。API の 3.x のこのワークフローに相当するのは、マップがロードされるとコールバックを実行する、`map.on('load', callback)` イベント ハンドラーを作成することです。 `view.when()` の使用例は <a href="https://developers.arcgis.com/javascript/latest/sample-code/overview-map/index.html" target="_blank">2D overview map sample</a> をご覧ください。

```js
require(["esri/Map", "esri/views/MapView", "dojo/domReady!"], function(Map, MapView){
  var map = new Map({
    basemap: 'streets'
  });
  var view = new MapView({
    container: "viewDiv",
    scale: 24000,
    map: map
  });
  view.when(function(instance){
    // ここにビューの読み込みが完了した後の動作のコードを書きます
  }, function(error){
    console.log('MapView promise rejected! Message: ', error);
  });
});
```

## 追加情報

<a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise" target="_blank">MDN Promise documentation</a> と <a href="http://dojotoolkit.org/reference-guide/1.10/dojo/promise/Promise.html#dojo-promise-promise" target="_blank">Dojo Promise documentation</a> では、promise の構造と使用方法をより詳細に理解することができます。以下は、promise に関する追加リンクです。

- <a href="https://www.sitepen.com/blog/2015/03/06/understanding-deferreds-and-promises-in-dojo/" target="_blank">Understanding Deferreds and Promises in Dojo</a>
- <a href="http://dojotoolkit.org/documentation/tutorials/1.10/promises/index.html" target="_blank">Dojo Deferreds and Promises</a>
- <a href="https://www.sitepen.com/blog/2015/06/10/dojo-faq-how-can-i-sequence-asynchronous-operations/" target="_blank">Chaining Promises</a>
- <a href="https://community.esri.com/people/odoe/blog/2015/06/17/keeping-promises" target="_blank">GeoNet - Keeping Promises</a> 
- <a href="http://www.html5rocks.com/en/tutorials/es6/promises/" target="_blank">JavaScript Promises</a>



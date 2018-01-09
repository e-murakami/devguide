
このページでは、写実的な 3D シンボルでポイント データを可視化する方法の概要を説明します。3D シンボルの概要については、3D シンボルと シンボル レイヤーの関係を説明する <a href="https://developers.arcgis.com/javascript/latest/guide/creating-visualizations-manually/index.html#symbols-3d" target="_blank">3D Symbols</a> をご覧ください。

## Web スタイル シンボル を使用する

<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-WebStyleSymbol.html" target="_blank">WebStyleSymbol</a> クラスを使用して、写実的、主題的な 3D シンボルを作成できます。このクラスは、API 内ですぐに利用できる数百個のシンボルへのアクセスを提供します。シンボルの <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-WebStyleSymbol.html#styleName" target="_blank">styleName</a> と <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-WebStyleSymbol.html#name" target="_blank">name</a> を参照するだけで、目的のビジュアライゼーションを作成することができます。

```js
var symbol = {
  type: "web-style",  // new WebStyleSymbol() としてオートキャスト
  styleName: "EsriRealisticStreetSceneStyle",
  name: "Light_On_Post_-_Light_on"
};
layer.renderer = {
  type: "simple",  // new SimpleRenderer() としてオートキャスト
  symbol: symbol
};
```

[![street-lamp](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/visualization3/street-lamp.png)](https://developers.arcgis.com/javascript/latest/sample-code/visualization-webstylesymbol/index.html)

有効な `styleNames` とそのシンボル名の完全なリストについては、<a href="https://developers.arcgis.com/javascript/latest/guide/esri-web-style-symbols/index.html" target="_blank">Esri Web Style Symbols</a> を参照してください。

## Web スタイル シンボルのサイズと色を変更する

<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-WebStyleSymbol.html" target="_blank">Web スタイル シンボル</a> には `size` や `color` のような典型的なシンボル プロパティはありません。Web スタイル シンボルは、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-ObjectSymbol3DLayer.html" target="_blank">ObjectSymbol3DLayer</a> または <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-IconSymbol3DLayer.html" target="_blank">IconSymbol3DLayer</a> のいずれかを含む <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-PointSymbol3D.html" target="_blank">PointSymbol3D</a> オブジェクトを作成するために使用されるラッパーと考えることができます。この目的のために、Web スタイル シンボルを表す <a href=""https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-PointSymbol3D.html target="_blank">PointSymbol3D</a> インスタンスのサイズと色のプロパティを変更できます。シンボルを表す <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-PointSymbol3D.html" target="_blank">PointSymbol3D</a> オブジェクトを取得するには、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-WebStyleSymbol.html#fetchSymbol" target="_blank">WebStyleSymbol.fetchSymbol()</a> メソッドを使用します。

```js
var symbol = {
  type: "web-style",  // new WebStyleSymbol() としてオートキャスト
  styleName: "EsriRealisticTransportationStyle",
  name: "Ambulance"
};
symbol.fetchSymbol()
  .then(function(ambulanceSymbol){
    var objectSymbolLayer = ambulanceSymbol.symbolLayers.getItemAt(0).clone();
    objectSymbolLayer.material = { color: "red" };
    objectSymbolLayer.height *= 2;
    objectSymbolLayer.width *= 2;
    objectSymbolLayer.depth *= 2;
    ambulanceSymbol.symbolLayers = [ objectSymbolLayer ];
    var renderer = layer.renderer.clone();
    renderer.symbol = ambulanceSymbol;
    layer.renderer = renderer;
  });
```

<a href="https://developers.arcgis.com/javascript/latest/sample-code/visualization-webstylesymbol/index.html" target="_blank">Visualize features with realistic WebStyleSymbols</a> サンプルは、このワークフローを示しています。

ビジュアル変数が Web スタイル シンボルを利用するレンダラーに適用される場合、Web スタイル シンボルは PointSymbol3D インスタンスに変換する必要はありません。 <a href="https://developers.arcgis.com/javascript/latest/sample-code/visualization-trees-realistic/index.html" target="_blank">Visualize features with realistic 3D symbols</a> サンプルは、Web スタイル シンボルを参照するレンダラーにビジュアル変数を適用して、実際の測定値に基づいて写実的なシンボルのサイズを調整する方法を示しています。


```js
// 実際の測定値に基づいて各シンボルのサイズを調整します。

var renderer = {
  type: "simple",  // new SimpleRenderer() としてオートキャスト
  symbol: {
    type: "web-style",  // new WebStyleSymbol() としてオートキャスト
    styleName: "esriRealisticTreesStyle",
    name: "Other"
  },
  label: "generic tree",
  visualVariables: [{
    type: "size",
    axis: "height",
    field: "Height",
    valueUnit: "feet"
  }, {
    type: "size",
    axis: "width-and-depth",
    field: "canopy_diameter",
    valueUnit: "feet"
  }]
};
```

## シンボルのカスタム

ArcGIS API for JavaScript ではカスタム 3D オブジェクトを作成することができないため、<a href="https://github.com/Esri/arcgis-pro-sdk-community-samples/tree/master/Map-Authoring/ExportWeb3DObjectResource#exportweb3dobjectresource" target="_blank">exporting a Web3DObjectResource</a> のインストラクションに従ってください。 Web3DObjectResource を作成したら、サーバー上にホストし、シンボル レイヤー リソースの <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-ObjectSymbol3DLayer.html#resource" target="_blank">href</a> プロパティをスタイルの URL に指定します。カスタム シンボルは <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-PointSymbol3D.html" target="_blank">PointSymbol3D</a> 内の <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-ObjectSymbol3DLayer.html" target="_blank">ObjectSymbol3DLayer</a> を使用した場合にのみ追加できます。

## シンボルとスタイルの名前

Web スタイル シンボルで使用できるすべてのスタイル名とシンボル名については、<a href="https://developers.arcgis.com/javascript/latest/guide/esri-web-style-symbols/index.html" target="_blank">Esri Web Style Symbols</a> をご参照ください。



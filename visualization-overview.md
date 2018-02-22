## イントロダクション

このガイドでは、次のいずれかのレイヤー タイプでフィーチャのビジュアライゼーションを定義または変更するために使用できるさまざまなワークフローの概要を説明します。  
<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html" target="_blank">フィーチャ レイヤー</a>｜<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-SceneLayer.html" target="_blank">シーン レイヤー</a>｜<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-MapImageLayer.html" target="_blank">マップ イメージ レイヤー</a>｜<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-CSVLayer.html" target="_blank">CSV レイヤー</a>｜<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-StreamLayer.html" target="_blank">ストリーム レイヤー</a>

![viz-overview](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/Visualization+Overview/viz-overview.jpg)


アプリケーションに表示したいジオメトリの種類（ポイント、ライン、ポリゴン、メッシュ）にかかわらず、データを表現する方法は以下の2つのシナリオのいずれかです。

- <b>場所のみ</b>：このシナリオでは、各フィーチャを表すシンボルの全ての視覚的なプロパティ（サイズ、色、不透明度、テクスチャーなど）は固定されています。この場合、ビジュアライゼーションの主な目的は、フィーチャの場所を示すことです。

- <b>テーマ別データドリブンのシンボル化</b>：各フィーチャのビジュアライゼーション（サイズ、色、不透明度、テクスチャーなど）は、次のいずれかのソースから返される属性データによって決まります。
    - <b>フィールド値</b>（＋正規化フィールド）：フィールド値はシンボルの視覚的なプロパティを決定します。フィールド値により作成されたビジュアライゼーションは、Web マップ/シーンで保持され、JSON にエクスポートされます。
    - <b><a href="https://developers.arcgis.com/javascript/latest/guide/arcade/index.html" target="_blank">Arcade</a> 式</b>：値を評価する式です。この式は ArcGIS Online または Portal for ArcGIS 上のアイテムに保存され共有することができます。Arcade 式で作成されたビジュアライゼーションは、Web マップ/シーンで保持され、JSON にエクスポートされます。
    - <b>JavaScript 関数</b>：値を返す関数です。関数で作成されたビジュアライゼーションは、Web マップ/シーンで保持されず、またJSON にエクスポートすることもできません。

場所のみおよびデータドリブン ビジュアライゼーションは、3 つの主要なワークフローのいずれかで作成されます。以下は、レイヤーのビジュアライゼーションを作成および変更するための手法を示しています。


## ArcGIS Online ツール

ArcGIS Online の<a href="https://www.arcgis.com/home/webmap/viewer.html" target="_blank">マップ ビューアー</a>と<a href="https://www.arcgis.com/home/webscene/viewer.html" target="_blank">シーン ビューアー</a> は、さまざまな方法で、レイヤーのビジュアライゼーションを変更するシンプルな UI を提供します。ユーザーは、単一のシンボル、またはレイヤーのフィールドや指定された Arcade 式で返された値に基づいた色、サイズ、不透明度、回転などを変更して、簡単にフィーチャを可視化できます。

[![twoattributes](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/Visualization+Overview/twoattributes.png)](https://blogs.esri.com/esri/arcgis/2016/12/01/how-to-smart-map-in-3-easy-steps/)

ArcGIS Online で利用できるレイヤー スタイリング ツールは、使用するレイヤーのデータによって異なるビジュアライゼーションを簡単に試すことができます。以下の <a href="https://blogs.esri.com/esri/arcgis/" target="_blank">ブログ記事</a> では、レイヤー アイテムと Web マップにビジュアライゼーションを作成して保存する方法について説明しています。

- 属性値を使用したビジュアライゼーション：<a href="https://blogs.esri.com/esri/arcgis/2016/12/01/how-to-smart-map-in-3-easy-steps/" target="_blank">How to Smart Map in 3 Easy Steps</a>
- Arcade 式を使用したビジュアライゼーション：<a href="https://blogs.esri.com/esri/arcgis/2016/12/15/use-arcade-expressions-to-map-your-ideas/" target="_blank">Use Arcade Expressions to Map Your Ideas</a>

ビジュアライゼーションはレイヤー アイテムとして保存されると、レイヤー アイテムからアプリケーションへ直接ビジュアライゼーションを読み込むことができます。

```js
var layer = new FeatureLayer({
  portalItem: {
    id: "d7892b3c13b44391992ecd42bfa92d01"
  }
});
map.add(layer);
```

以下は Web マップ アイテムまたは Web シーン アイテムの場合です。

```js
// Web マップを読み込みます
var webmap = new WebMap({
  portalItem: { //new PortalItem()としてオートキャスト
    id: "e691172598f04ea8881cd2a4adaa45ba"
  }
});
// MapView の map プロパティに WebMap インスタンスをセットします
var view = new MapView({
  map: webmap,
  container: "viewDiv"
});
```

以下は Web マップ、Web シーン、または ArcGIS Online からポータル ID を使って直接レイヤーを読み込む方法を示したサンプルです。

- <a href="https://developers.arcgis.com/javascript/latest/sample-code/layers-portal/index.html" target="_blank">Create a layer from a portal item</a>
- <a href="https://developers.arcgis.com/javascript/latest/sample-code/webmap-basic/index.html" target="_blank">Load a basic web map</a>
- <a href="https://developers.arcgis.com/javascript/latest/sample-code/webscene-basic/index.html" target="_blank">Load a basic web scene</a>

## スマート マッピング API

ArcGIS Online のビジュアライゼーション ツールは、データセットの統計情報およびベースマップに基づいた「スマート」なデフォルト シンボルを使用して、色、サイズ、またはその両方をもとにレンダラーを生成するためのいくつかの API の上に構築されています。これらの API および API を使用して生成されたビジュアライゼーションは<a href="https://blogs.esri.com/esri/arcgis/2015/03/02/introducing-smart-mapping/" target="_blank">スマート マッピング</a>と呼ばれます。次のオブジェクトには、スマート デフォルトでビジュアライゼーションを生成するメソッドが含まれています。

- <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-smartMapping-creators-color.html" target="_blank">colorRendererCreator</a>
- <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-smartMapping-creators-size.html" target="_blank">sizeRendererCreator</a>
- <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-smartMapping-creators-location.html" target="_blank">locationRendererCreator</a>
- <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-smartMapping-creators-univariateColorSize.html" target="_blank">univariateColorSizeRendererCreator</a>

これらのオブジェクトのメソッドは、次のスライダー ウィジェットと組み合わせて使用できます。

- <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-ColorSlider.html" target="_blank">ColorSlider</a>
- <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-SizeSlider.html" target="_blank">SizeSlider</a>
- <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-UnivariateColorSizeSlider.html" target="_blank">UnivariateColorSizeSlider</a>

[![color-slider](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/Visualization+Overview/color-slider.png)](https://developers.arcgis.com/javascript/latest/sample-code/visualization-sm-color/index.html)

一般的に、これらのメソッドはほとんどのプロダクション アプリで使用すべきではありません。むしろ、以下の操作を行うことができるアプリを作成するために、アプリ開発者がレンダラー生成 API とスライダー ウィジェットを使用できる、という特定のユースケースに合わせて設計されています。

- <b>レイヤー内の未詳のデータを探索します。</b> ほとんどの場合、ArcGIS Online のツールではすでにユーザーがこれを行うことができます。ただし、アプリケーションを作成して、データの探索のための動作をカスタマイズすることができます。この例については、<a href="https://blogs.esri.com/esri/arcgis/2016/03/28/using-smart-mapping-in-custom-web-apps/" target="_blank">ブログ記事</a>を参照してください。
- フィールド値に基づいた<b>レイヤーのビジュアライゼーションを作成またはデザインします。</b>アプリはユーザーがビジュアライゼーションをポータル アイテムに保存する方法や、JSON としてエクスポートして他のカスタム アプリケーションで読み込む方法を提供します。

以下のサンプルは、レンダラーを作成する関数をスライダー ウィジェットと一緒に使用する方法を示しています。

- <a href="https://developers.arcgis.com/javascript/latest/sample-code/visualization-sm-color/index.html" target="_blank">Generate continuous color visualization</a>
- <a href="https://developers.arcgis.com/javascript/latest/sample-code/visualization-sm-size/index.html" target="_blank">Generate continuous size visualization</a>
- <a href="https://developers.arcgis.com/javascript/latest/sample-code/visualization-sm-uni-colorsize/index.html" target="_blank">Generate univariate continuous size and color visualization in 3D</a>
- <a href="https://developers.arcgis.com/javascript/latest/sample-code/visualization-sm-multivariate/index.html" target="_blank">Multivariate data exploration</a>

## 手動で定義されたレンダラー

<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-Renderer.html" target="_blank">renderer</a> クラス を使用してレイヤーのビジュアライゼーションをプログラムで定義または変更することもできます。<a href="https://developers.arcgis.com/javascript/latest/guide/creating-visualizations-manually/index.html" target="_blank">Creating visualizations manually</a> で、API で使用可能なレンダラー、それらの構築方法、およびそれらが設計されたさまざまなユースケースを概説します。

レンダラーのシンボルやその他のレンダラー プロパティを変更するためには、まずレンダラーをクローンしてプロパティを変更し、新しいレンダラーをレイヤーに適用する必要があります。

```js
// レンダラーをクローン
var renderer = layer.renderer.clone();
// 凡例の中のレンダラーのタイトルを変更
renderer.legendOptions = {
  title: "Population density"
};
// 変更したレンダラーをレイヤーに適用
layer.renderer = renderer;
```

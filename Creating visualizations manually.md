<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html" target="_blank">フィーチャ レイヤー</a> または <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-StreamLayer.html" target="_blank">ストリーム レイヤー</a> のフィーチャのビジュアライゼーションをプログラムによって変更するサンプルを以下に示します。<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-GraphicsLayer.html" target="_blank">グラフィック レイヤー</a> の可視化は、個々の <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-Graphic.html" target="_blank">グラフィックス</a> (<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-GraphicsLayer.html" target="_blank">グラフィック レイヤー</a> は <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-Renderer.html" target="_blank">レンダラー</a> を持たない）のシンボルによって決定されます。


## ビジュアライゼーション サンプルの概要

### 場所

場合によっては、ユーザーはフィーチャの場所のみを知りたいことがあります。次のサンプルは、場所のみを知りたい場合のさまざまな方法を示しています。

### すべてのフィーチャを同じシンボルで可視化する

このサンプルは、レイヤー内の全てのフィーチャに同じシンボルを割り当てる方法を示しています。これは最も基本的なビジュアライゼーションであり、属性データによって駆動されるものではありません。これは、境界、道路、興味のあるポイント、プロジェクト エリア等を表示するなど、単に文脈上の理由でレイヤーを追加する場合に便利です。

[![1-simple-sym.png](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/Creating+visualizations+manually/1-simple-sym.png)](https://developers.arcgis.com/javascript/latest/sample-code/visualization-location-simple/index.html)

### 実際のサイズ（2D）に基づいてフィーチャのサイズを調整する

単純な場所の他にもっと情報をフィーチャに追加したいと思うかもしれません。このサンプルでは、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-SimpleRenderer.html#visualVariables" target="_blank">ビジュアル変数</a>
 を使用して 2D <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html" target="_blank">MapView</a>
 で林冠の実際のサイズに基づいたポイント シンボルを作成する方法を示します。

[![4-trees-2d](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/Creating+visualizations+manually/4-trees-2d.png)]( https://developers.arcgis.com/javascript/latest/sample-code/visualization-trees-2d/index.html)


### リアルな 3D シンボルを使用したフィーチャの可視化

前出のサンプルをさらに進化させて、実際のフィーチャの外観に似た 3D シンボルを<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html" target="_blank"> SceneView</a> で作成することができます。ここでも、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-SimpleRenderer.html#visualVariables" target="_blank">ビジュアル変数</a> は、ポイントとして格納された木のフィーチャの測定値および大きさを含む数値属性フィールドを参照するために使用されます。レンダラーは <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-SimpleRenderer.html#visualVariables" target="_blank">ビジュアル変数</a> を使用して、各シンボルの異なる部分を比例させて、実物の木のような 3D 表現 で再現します。

[![3-trees-3d](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/Creating+visualizations+manually/3-trees-3d.png)]( https://developers.arcgis.com/javascript/latest/sample-code/visualization-trees-realistic/index.html)

### 回転によるデータの可視化

このサンプルは、回転を使用してポイント データを可視化する方法を示しています。市営バス フィーチャは <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-StreamLayer.html" target="_blank">ストリーム レイヤー</a> からロードされ、各バスの方向によって回転する矢印シンボルが割り当てられます。回転はポイント フィーチャにのみ適用されます。

[![10-vv-rotation](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/Creating+visualizations+manually/10-vv-rotation.png)](https://developers.arcgis.com/javascript/latest/sample-code/visualization-vv-rotation/index.html)

### 3D でのポイントのビジュアライゼーション

このサンプルは、次の問題を解決するための機能を示しています。
<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html#elevationInfo" target="_blank">relative-to-scene</a>, <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-PointSymbol3D.html#callout" target="_blank">callout</a>, <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-PointSymbol3D.html#verticalOffset" target="_blank">verticalOffset</a>, <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html#featureReduction" target="_blank">decluttering</a> ,<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html#screenSizePerspectiveEnabled" target="_blank">screenSizePerspectiveEnabled</a>

[![14-city-points-callout](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/Creating+visualizations+manually/14-city-points-callout.png)](https://developers.arcgis.com/javascript/latest/sample-code/visualization-point-styles/index.html)

## タイプ

以下のサンプルはフィーチャが持つ固有のタイプによるビジュアライゼーションを示しています。タイプは通常、文字列属性フィールドに基づいており、色で可視化されます。

### タイプごとにフィーチャを可視化する

このサンプルは、文字列フィールドの値に基づいてレイヤーのフィーチャを可視化します。タイプごとに可視化することは、道路タイプ（住宅地の道路、幹線道路、高速道路など）のようなはっきりわかる分類を持つデータの可視化に役立ちます。

[![5-types](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/Creating+visualizations+manually/5-types.png)](https://developers.arcgis.com/javascript/latest/sample-code/visualization-location-types/index.html)

### 実際の高さに基づいて建物のフットプリントを立ち上げる

このサンプルは、各フィーチャを数値フィールドで立ち上げ、3D <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html" target="_blank">SceneView</a> でポリゴン フィーチャをタイプ別に可視化する方法を示します。建物の場所をタイプ別（住居、商業、ホテルなど）に見たい場合に使用します。色によるフィーチャの可視化に加えて、各建物の高さに基づいて各建物を立ち上げることで、各建物の情報をより詳細に把握することができます。

[![2-extrude-height](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/Creating+visualizations+manually/2-extrude-height.png)](https://developers.arcgis.com/javascript/latest/sample-code/visualization-buildings-3d/index.html)


## 数と量

以下のサンプルは、人口数や風速、割合などの数値データを可視化する方法を示しています。

### クラス区切りによるデータの可視化

変数に対して明確に定義されたクラス区切りがすでにわかっている場合は、区切りごとに別々のシンボルを定義するのが適切です。

[![6-classbreaks](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/Creating+visualizations+manually/6-classbreaks.png)](https://developers.arcgis.com/javascript/latest/sample-code/visualization-classbreaks/index.html)

### 連続カラー

データ区切りが明確ではない、または判別が難しい場合、連続カラーを使用して 2つ以上のカラー ランプに沿って数値属性を可視化することが適しています。連続カラー ランプは、特にポリゴン フィーチャのデータ分布について明らかにすることに適しています。

[![7-vv-color](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/Creating+visualizations+manually/7-vv-color.png)]( https://developers.arcgis.com/javascript/latest/sample-code/visualization-vv-color/index.html)

### 連続サイズ

このサンプルは、連続したサイズと 2D シンボルで数値属性データを可視化する方法を示しています。人口統計データを含む米国のポリゴン フィーチャを参照しています。ポリゴンに <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-MarkerSymbol.html" target="_blank">マーカー シンボル</a>
 を割り当てて、連続したサイズのデータを可視化することができます。

[![simpleUI](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/Creating+visualizations+manually/8-vv-size.png)]( https://developers.arcgis.com/javascript/latest/sample-code/visualization-vv-size/index.html)


### データ駆動の立ち上げ

上のサンプルと同様に、このサンプルは連続したサイズで数値データを可視化する方法を示しています。しかし、ここでは、3D <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html" target="_blank">SceneView</a> で立ち上げを使用して人口統計を表現しています。立ち上げは、ポリゴン ジオメトリを持つフィーチャ、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-ObjectSymbol3DLayer.html" target="_blank">ObjectSymbol3DLayers</a> を使用するフィーチャに適用されます。

[![9-vv-size-3d](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/Creating+visualizations+manually/9-vv-size-3d.png)](https://developers.arcgis.com/javascript/latest/sample-code/visualization-vv-extrusion/index.html)

### データ駆動の不透明度

不透明度を使用して、一部のフィーチャの重要性を低減または強化することができます。このサンプルは、国勢調査ブロック群内の最も一般的な最終学歴を示すレイヤーを表現しています。不透明感がレンダラーに追加され、数値フィールドの値に基づいたフィーチャ（ブロック群内の全人口）に適用されます。これは人口が少ないフィーチャをトーン ダウンし、人口が多いフィーチャに注意を引き付けます。

[![12-vv-transparency](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/Creating+visualizations+manually/12-vv-transparency.png)](https://developers.arcgis.com/javascript/latest/sample-code/visualization-vv-opacity/index.html)

### 二変数または多変数の可視化（2D）

<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-SimpleRenderer.html#visualVariables" target="_blank">ビジュアル変数</a> を使用すると、少量のコードで二変数または、多変数のビジュアライゼーションをプログラムで作成することができます。複数の変数を可視化すると、2つ以上の変数の空間的な傾向を明らかにすることができます。このサンプルは、サイズと色を組み合わせて、2つの数値フィールド間の潜在的な関係を考察する方法を示しています。

[![13-multivariate-2d](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/visualization3/13-multivariate-2d.png)]( https://developers.arcgis.com/javascript/latest/sample-code/visualization-multivariate-2d/index.html)

### 二変数または多変数の可視化（3D）

上のサンプルと同様に、このサンプルは、 <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-Renderer.html#ColorVisualVariable" target="_blank">色</a>
 と <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-Renderer.html#SizeVisualVariable" target="_blank">サイズ</a>
 の <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-SimpleRenderer.html#visualVariables" target="_blank">ビジュアル変数</a>
 を使用して、2つの数値フィールドの多変量のビジュアライゼーションを作成する方法を示しています。こちらのサンプルでは、３D シンボルがデータに適用され、より美しいビジュアライゼーションを作成しています。

 [![11-multivariate-3d](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/visualization3/11-multivariate-3d.png)]( https://developers.arcgis.com/javascript/latest/sample-code/visualization-multivariate-3d/index.html)

 
## レンダラーの概要

<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html" target="_blank">フィーチャ レイヤー</a> のビジュアライゼーションは <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html#renderer" target="_blank">レンダラー</a> によって決定されます。フィーチャ レイヤーには 次の 3種類のレンダラーを適用できます。

- <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-SimpleRenderer.html" target="_blank">SimpleRenderer</a>
- <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-UniqueValueRenderer.html" target="_blank">UniqueValueRenderer</a>
- <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-UniqueValueRenderer.html" target="_blank">ClassBreaksRenderer</a>

各レンダラーには <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-SimpleRenderer.html#visualVariables" target="_blank">visualVariables</a> プロパティがあり、数値フィールドに基づいて、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-Renderer.html#ColorVisualVariable" target="_blank">色</a>、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-Renderer.html#SizeVisualVariable" target="_blank">サイズ</a>、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-Renderer.html#OpacityVisualVariable" target="_blank">不透明度</a>、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-Renderer.html#RotationVisualVariable" target="_blank">回転</a>、または 2 つ以上の変数の組合せによるシンプルなデータ駆動型のビジュアライゼーションを定義できます。レンダラーとビジュアル変数は、2D（<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html" target="_blank">MapViews</a>）と 3D（<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html" target="_blank">SceneViews</a>）の両方のジオメトリタイプのレイヤーに適用できます。フィーチャ レイヤーにレンダラーを適用する方法については、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-Renderer.html" target="_blank">Renderer</a> のドキュメントをご覧ください。

## シンボルの概要

<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-Symbol.html" target="_blank">シンボル</a>は 個々の <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-Graphic.html" target="_blank">フィーチャ</a> のビジュアライゼーションを定義し、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-Renderer.html" target="_blank">レンダラー</a> が <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-View.html" target="_blank">ビュー</a> でレイヤーを描画する方法を決定するときに使用します。シンボルには、<a href="https://developers.arcgis.com/javascript/latest/guide/creating-visualizations-manually/index.html#2d-symbols" target="_blank">2D シンボル</a>と<a href="https://developers.arcgis.com/javascript/latest/guide/creating-visualizations-manually/index.html#symbols-3d" target="_blank"> 3D シンボル</a> の 2 種類があります。ハンズオン環境でシンボルを構築する方法について学ぶには <a href="https://developers.arcgis.com/javascript/latest/sample-code/playground/index.html" target="_blank">Symbol Playground</a> をご参照ください。

## 2D シンボル

下の表に示す 2D シンボルを使用して、2D <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html" target="_blank">MapView </a>または 3D <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html" target="_blank">SceneView</a> のフィーチャを可視化することができます。しかしながら、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html" target="_blank">MapView </a>で追加されたレイヤーでのみ使用することをおすすめします。シンボルには、レイヤー内のフィーチャのビジュアライゼーションを定義する <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-SimpleMarkerSymbol.html#color" target="_blank"> color </a>、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-SimpleMarkerSymbol.html#size" target="_blank"> size </a>、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-SimpleMarkerSymbol.html#outline" target="_blank"> outline </a>（シンボル タイプに依存）などのプロパティが含まれています。レンダラー上の <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-SimpleRenderer.html#visualVariables" target="_blank">ビジュアル変数</a> を使用して、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-SimpleMarkerSymbol.html#color" target="_blank"> color </a> と <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-SimpleMarkerSymbol.html#size" target="_blank"> size </a> のプロパティを変更することができます。

|シンボル  |ジオメトリ  |
|---|---|
|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-SimpleMarkerSymbol.html" target="_blank">SimpleMarkerSymbol</a>  |<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Point.html" target="_blank">Point</a>, <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Polygon.html" target="_blank">Polygon</a>  |
|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-PictureMarkerSymbol.html" target="_blank">PictureMarkerSymbol</a>  |<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Point.html" target="_blank">Point</a>, <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Polygon.html" target="_blank">Polygon</a>  |
|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-SimpleLineSymbol.html" target="_blank">SimpleLineSymbol</a>  |<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Polyline.html" target="_blank">Polyline</a>  |
|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-SimpleFillSymbol.html" target="_blank">SimpleFillSymbol</a>  |<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Polygon.html" target="_blank">Polygon</a>  |
|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-PictureFillSymbol.html" target="_blank">PictureFillSymbol</a>  |<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Polygon.html" target="_blank">Polygon</a>  |
|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-TextSymbol.html" target="_blank">TextSymbol</a>  |<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Point.html" target="_blank">Point</a>,<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Polyline.html" target="_blank">Polyline</a>, <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Polygon.html" target="_blank">Polygon</a> |

詳細は <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-Symbol.html" target="_blank"> シンボル </a> およびそのサブクラスのドキュメントを参照してください。

## 3D シンボル

下の表に示す 3D シンボルは、3D <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html" target="_blank">SceneView</a> のフィーチャを可視化するためにのみ使用できます。2D のシンボルとは異なり、3D シンボルは <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-Symbol3DLayer.html" target="_blank">シンボル レイヤー</a> のコンテナです。1 つまたは複数のシンボル レイヤーが3D シンボルのビジュアライゼーションを決定します。3D シンボルには、フィーチャが表示されるためのシンボル レイヤーが少なくとも 1 つ必要です。1 つ以上のシンボル レイヤーを使用して 3D シンボルを定義することができますが、レンダラーに <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-SimpleRenderer.html#visualVariables" target="_blank">ビジュアル変数</a> を適用するときは、1 つのシンボル レイヤーしかサポートされません。下の表は、各シンボル レイヤーと、シンボル タイプと、それぞれが適用されるジオメトリを示しています。

|シンボル レイヤー|シンボル|ジオメトリ|説明|サイズ|例| 
|---|---|---|---|---|---|
|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-IconSymbol3DLayer.html" target="_blank">IconSymbol3DLayer</a>|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-PointSymbol3D.html" target="_blank">PointSymbol3D</a>,<br><a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-PolygonSymbol3D.html" target="_blank">PolygonSymbol3D</a>|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Point.html" target="_blank">Point</a>, <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Polygon.html" target="_blank">Polygon</a>|平面|ポイント/ピクセル|![symbols3d-icon-circle](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/visualization3/symbols3d-icon-circle.png)|
|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-ObjectSymbol3DLayer.html" target="_blank">ObjectSymbol3DLayer</a>|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-PointSymbol3D.html" target="_blank">PointSymbol3D</a>,<br><a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-PointSymbol3D.html" target="_blank">PolygonSymbol3D</a>|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Point.html" target="_blank">Point</a>, <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Polygon.html" target="_blank">Polygon</a>|立体|メートル|![ symbols3d-object-sphere](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/visualization3/symbols3d-object-sphere.png)|
|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-LineSymbol3DLayer.html" target="_blank">LineSymbol3DLayer</a>|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-LineSymbol3D.html" target="_blank">LineSymbol3D</a>, <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-PointSymbol3D.html" target="_blank">PolygonSymbol3D</a>|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Polyline.html" target="_blank">Polyline</a>, <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Polygon.html" target="_blank">Polygon</a>|平面|ポイント/ピクセル|![symbols3d-line-line](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/visualization3/symbols3d-line-line.png)|
|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-PathSymbol3DLayer.html" target="_blank">PathSymbol3DLayer</a>|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-LineSymbol3D.html" target="_blank">LineSymbol3D</a>|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Polyline.html" target="_blank">Polyline</a>|立体|メートル|![https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/visualization3/symbols3d-path-tube](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/visualization3/symbols3d-path-tube.png)|
|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-FillSymbol3DLayer.html" target="_blank">FillSymbol3DLayer</a>|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-PointSymbol3D.html" target="_blank">PolygonSymbol3D</a>, <br><a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-MeshSymbol3D.html" target="_blank">MeshSymbol3D</a>|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Polygon.html" target="_blank">Polygon</a>, Mesh|平面|-|![ symbols3d-fill-solid](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/visualization3/symbols3d-fill-solid.png)|
|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-ExtrudeSymbol3DLayer.html" target="_blank">ExtrudeSymbol3DLayer</a>|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-PointSymbol3D.html" target="_blank">PolygonSymbol3D</a>|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Polygon.html" target="_blank">Polygon</a>|立体|メートル|![ symbols3d-extrude-solid](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/visualization3/symbols3d-extrude-solid.png)|
|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-TextSymbol3DLayer.html" target="_blank">TextSymbol3DLayer</a>|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-LabelSymbol3D.html" target="_blank">LabelSymbol3D</a>|<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Point.html" target="_blank">Point</a>,<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Polyline.html" target="_blank">Polyline</a>,<br><a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-Polygon.html" target="_blank">Polygon</a>|平面|ポイント/ピクセル|![ symbols3d-label-text](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/visualization3/symbols3d-label-text.png)|

シンボル レイヤーとシンボルの関係については <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-Symbol3DLayer.html" target="_blank">Symbol3DLayer</a> および <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-Symbol3D.html" target="_blank">Symbol3D</a> のドキュメントを参照してください。


## 他のリソース

- <a href="https://developers.arcgis.com/javascript/latest/sample-code/playground/index.html" target="_blank">Symbol Playground</a>
- <a href="https://developers.arcgis.com/javascript/latest/guide/visualizing-points-3d/index.html" target="_blank">Visualizing points with 3D symbols</a>
- <a href="https://blogs.esri.com/esri/arcgis/2016/01/19/3d-visualization-working-with-icons-lines-and-fill-symbols/" target="_blank">ArcGIS blog - Working with icons, lines, and fill symbols</a>
- <a href="https://blogs.esri.com/esri/arcgis/2016/01/25/3d-visualization-working-with-objects-paths-and-extrusion/" target="_blank">ArcGIS blog - Working with objects, paths, and extrusion</a>
- <a href="https://blogs.esri.com/esri/arcgis/2016/02/01/3d-visualization-using-attributes-to-represent-real-world-sizes-of-features/" target="_blank">ArcGIS blog - Using attributes to represent real-world sizes of features</a>


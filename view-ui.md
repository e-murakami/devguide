ArcGIS API for JavaScript 4.x は、シンプルな UI レイアウトを設定するためのインターフェースを提供します。<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-View.html#ui" target="_blank">view</a> はコンポーネント（ウィジェットまたは DOM 要素）をコーナーに配置できるようにする UI API を提供します。

以下のアプリケーションを例にしてみましょう。

[![simpleUI](https://s3-ap-northeast-1.amazonaws.com/apps.esrij.com/arcgis-dev/guide/img/js_devguid/simple-ui.png)](https://developers.arcgis.com/javascript/latest/sample-code/simple-ui/)

ポジショニングを定義する CSS は、一般的に次のようになります。

```js
.search,
.logo {
  position: absolute;
  right: 15px;
}
.search {
  top: 15px;
}
.logo {
  bottom: 30px;
}
```

この CSS でも動作しますが、コンポーネントが増えたときや、ウィジェットや view の大きさを変更してポジショニングの CSS の更新を必要とする場合は、維持するのが面倒になることがあります。

UI API を使用すると、これらのポジショニングの問題を回避することができます。上のコードの CSS は UI API を使用する場合は不要で、以下のコードに置き換えられます。

```js
view.ui.add(search, "top-right");
view.ui.add(logo, "bottom-right");
```

これにより、コンポーネントが上部および下部の右端に配置されます。たくさんのコンポーネントを追加することができ、位置と間隔は自動的に調整されます。詳細については、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-ui-UI.html#add" target="_blank">add() メソッド</a> を参照してください。

デフォルトでは、MapView または SceneView のいずれかで使用できるウィジェット コンポーネントがあります。デフォルト ウィジェットは文字列の配列として表され、<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-ui-DefaultUI.html#components" target="_blank">DefaultUI クラスの components プロパティ</a> に記述されています。配列を変更して、アプリケーションで使用したいウィジェットのみを表示することができます。

```js
view.ui.components = (["attribution", "compass", "zoom"]);
view.ui.components = (["attribution"]);
```

アプリケーションでコンポーネントの位置を動的に変更する必要がある場合は、UI を使用してコンポーネントを<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-ui-UI.html#move" target="_blank">移動</a>および<a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-ui-UI.html#remove" target="_blank">削除</a>することもできます。

```js
view.ui.move(logo, "bottom-left");
view.ui.remove(search);
view.ui.remove([compass, "zoom"]);
```

## 関連リンク

- <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-ui-UI.html" target="_blank">UI</a> クラス
- <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-views-ui-DefaultUI.html" target="_blank">DefaultUI</a> クラス
- <a href="https://developers.arcgis.com/javascript/latest/sample-code/simple-ui/" target="_blank">Simple UI</a>
- <a href="https://developers.arcgis.com/javascript/latest/sample-code/view-padding/" target="_blank">View padding</a>
- <a href="https://developers.arcgis.com/javascript/latest/sample-code/frameworks-bootstrap/" target="_blank">Create a Calcite Maps Bootstrap application</a>

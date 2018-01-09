ArcGIS API for JavaScript はすべてのプロパティを取得および設定するシンプルで一貫した方法をサポートしています。

> 各クラスの全てのプロパティはコンストラクターで設定できます。詳細については、<a href="https://developers.arcgis.com/javascript/latest/guide/discover/index.html#constructors-and-properties" target="_blank">Constructors and properties</a> を参照してください。 

## プロパティの取得

プロパティの値にアクセスする方法の 1 つは、オブジェクトから直接参照することです。この方法を使用して、マップのベースマップのタイトルを取得する方法については、以下の例をご覧ください。

```js
var basemapTitle = null;
// ベースマップのタイトルを取得する前にベースマップの存在をチェック
if (map.basemap) {
  basemapTitle = map.basemap.title;
}
```

`get()` 関数を使用してプロパティを取得することもできます。`get()` 関数は目的のプロパティを取得する前にオブジェクトが存在するかどうかを自動的にチェックするため、自分でチェックする必要はありません。上の例では`if (map.basemap)` を使用してオブジェクトが存在するかチェックしました。下のコードでは `get()` 関数を使用してオブジェクトが存在するかチェックします。

```js
var basemapTitle = map.get("basemap.title");
```

## プロパティの設定

プロパティの値を設定するには、以下のようにオブジェクトに直接設定することができます。

```js
view.center = [ -100, 40 ];
view.zoom = 6;
map.basemap = 'oceans';
```

また、`set()` メソッドを呼び出すことで、複数のプロパティを同時に設定することもできます。

```js
var viewProperties = {
  center: [ -100, 40 ],
  zoom: 6
};
view.set(viewProperties);
```
>4.x より前のバージョンでは、`getMethodname()` または `setMethodname()` を呼び出すことによって、一部のプロパティを取得（読取り）または設定（書込み）することができました。4.x では、API がすべてのプロパティを取得して設定するシンプルで一貫した方法をサポートするため、これらのメソッドは必要ありません。

### プロパティの監視

>4.ｘ 以前のバージョンでは、プロパティの変更の監視はイベントを介して処理されていました。

プロパティの変更の監視は、 `.watch(property, callback)` メソッドで処理されます。監視されるプロパティが変更されるたびにコールバックが呼び出されます。 `basemap.title` のようにネストされたプロパティを監視することもできます。

例えば、下のコードのように、マップのベースマップのタイトルの変更を監視するハンドラを設定します。

```js
//  'streets' のベースマップを使用して、新しいマップを作成します。
var map = new Map({
  basemap: 'streets'
});

// watch ハンドラ: マップのベースマップのタイトルが変更されるたびにコールバックが実行されます。
var handle = map.watch('basemap.title', function(newValue, oldValue, property, object) {
 console.log("New value: ", newValue,      // 新しいプロパティの値
             "<br>Old value: ", oldValue,  // 変更されたプロパティの以前の値
             "<br>Watched property: ", property,  // この例では、この値は常に "basemap.title"
             "<br>Watched object: ", object);     // この例では、この値は常に map object
});
```

もしユーザーがベースマップを以下のように変更した場合

```js
map.basemap = "topo";
```

コンソールにイベント ハンドラに基づいて以下の情報を記録します。

```js
New value: Topographic
Old value: Streets
Watched property: basemap.title
Watched object: //これはマップオブジェクトを記録します
```

全てのプロパティを監視できるわけではありません。 <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-core-Collection.html" target="_blank">Collection</a> タイプの全てのプロパティは監視することができません。監視する代わりに、イベント ハンドラを登録して、コレクションの変更を通知することができます。詳細は <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-core-Collection.html#event:change" target="_blank">change</a> イベント ドキュメントをご覧ください。

# 追加情報

詳細については、次のリンク(英文）を参照してください。

- <a href="https://developers.arcgis.com/javascript/latest/guide/migrating/index.html#properties" target="_blank">Migrating from 3.x to 4.6</a>
- <a href="https://developers.arcgis.com/javascript/latest/api-reference/esri-core-Accessor.html" target="_blank">Accessor class</a>


# comic-search

## About
マンガ喫茶の在庫を一括検索するためのツールです。以下の2サイトに対応してます。
- www2.uraraka-comic.com
- www.navi-comi.com

## Usage
```javascript
npm install
index.js
```

## 設計
### 入出力形式
- 設定はjson5形式で用意
    - comics.json5
    - stores.json5
- 結果はcsvで出力
    - result.csv

### comics.json5
```json5
[
  {
    name: "鬼灯の冷徹", //csvのヘッダに表示する店舗名
    query: ["鬼灯"] //店舗によってコミック名の管理が異なる際の対策クエリ
  },{
  }
]
```

### stores.json5
```
[
  {
    name: "まんが広場", //csvのヘッダに表示する店舗名
    type: "uraraka", //uraraka or navi-comi
    id: 000 //POST時のtcdパラメータ
  },{
    name: "MANBOO",
    type: "navi-comi",
    id: 00000 //GET時のURL(www.navi-comi.com/00000/comics/...)
  }
]
```

### result.csv
```csv
,まんが広場,MANBOO
鬼灯の冷徹,○,×
```

## 実装方針
1. node-configで設定ファイル読み込み
1. 1店舗1秒より速くリクエストしない
    1. 異なる店舗なら並列実行OKとする
1. comics.nameで検索
    1. 結果が1件なら「○」、2件以上なら「？」を出力、0件ならqueryを使ってcomics.queriesを使って再検索
    1. すべてのクエリで検索した結果(重複除く)が1件なら「○」、2件以上なら「？」、0件なら「×」を出力する
1. すべての店舗・すべてのマンガの組み合わせで上記を繰り返す

## TODO
- babel使ってみる？

# 不動産情報検索

## 大枠の概念
RDBと比較すると大体以下の概念が当てはまる

|Elasticsearch|RDB|
|:--|:--|
|index|database|
|type|table|
|document|record|


### index作成
```
curl -X PUT http://localhost:9200/houses/\?pretty
```

### 作成したindexを確認する
```
curl -X GET http://localhost:9200/houses/\?pretty
```

### （おまけ）index削除
```
curl -X DELETE http://localhost:9200/houses/
```

### type作成
elasticsearch6からはtypeの指定は**非推奨**で_docを使うように。とのこと  
参考　https://qiita.com/nskydiving/items/1c2dc4e0b9c98d164329

なのでindexにdocumentをガンガン突っ込むイメージかな？


### document作成
作成したdocumentのjsonを読み込んで送信します。
```
curl -X POST -H "Content-Type: application/json" -d @real_estate/json/document1.json http://localhost:9200/houses/_doc/1\?pretty

curl -X POST -H "Content-Type: application/json" -d @real_estate/json/document2.json http://localhost:9200/houses/_doc/2\?pretty
```

### documentを検索する

housesの中を全部検索
```
curl -X POST -H "Content-Type: application/json" http://localhost:9200/houses/_search\?pretty
```

housesの中でnameを指定して検索
```
curl -X POST -H "Content-Type: application/json" http://localhost:9200/houses/_search\?pretty -d '{"query": {"match": {"name": "カイバヒルズ"}}}'
```

AND検索
```
curl -X POST -H "Content-Type: application/json" http://localhost:9200/houses/_search\?pretty -d '{"query": {"bool": {"must": [{"match": {"name": "カイバヒルズ"}}, {"match": {"description": "夜景"}}]}}}'
```

```json
{
    "query": {
        "bool": {
            "must": [
                {
                    "match": {
                        "name": "カイバヒルズ"
                    }
                },
                {
                    "match": {
                        "description": "夜景"
                    }
                }
            ]
        }
    }
}
```


OR検索
```
curl -X POST -H "Content-Type: application/json" http://localhost:9200/houses/_search\?pretty -d '{"query": {"bool": {"should": [{"match": {"name": "カイバヒルズ"}}, {"match": {"description": "温泉"}}]}}}'
```

```json
{
    "query": {
        "bool": {
            "should": [
                {
                    "match": {
                        "name": "カイバヒルズ"
                    }
                },
                {
                    "match": {
                        "description": "温泉"
                    }
                }
            ]
        }
    }
}
```


ソートして結果を取得
```
curl -X POST -H "Content-Type: application/json" http://localhost:9200/houses/_search\?pretty -d '{"sort": [{"created_at": "desc"}]}'
```

範囲指定して結果を取得
```
curl -X POST -H "Content-Type: application/json" http://localhost:9200/houses/_search\?pretty -d '{"query": {"range": {"stations.minute_on_foot": {"lte": 2}}}}'
```

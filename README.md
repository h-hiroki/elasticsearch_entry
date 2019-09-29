# elasticsearch_entry
elasticsearch入門+を7.3.2で行う

# コマンドライン操作メモ

起動している事を確認する
```
curl http://localhost:9200
```

インストールされているプラグインを確認する
```
curl http://localhost:9200/_nodes/plugins\?pretty
```


# Debug
binding.pry

## Error
```
ActiveRecord::StatementInvalid:
  Mysql2::Error: Table 'shimo-api-server_test.food_categories' doesn't exist: SELECT `food_categories`.* FROM `food_categories`
```

testのテーブルがないっぽいので、作る必要がある

```
$ ./bin/rails db:test:load ( schema.rb ファイルを基にtest環境のDBを構築 )
```


他にもいろいろある

```
$ ./bin/rails db:test:clone
現在使用中環境のDBスキーマよりtest環境のDBを構築
```

```
$ ./bin/rails db:test:clone_structure
development 環境のDBスキーマを使ってtest環境のDBを構築
```

```
$ ./bin/rails db:test:load
schema.rb ファイルを基にtest環境のDBを構築
```

```
$ ./bin/rails db:test:prepare
未反映のマイグレーションがないことを確認してから db:test:load実行
```

```
$ ./bin/rails db:test:purge
test環境のDBを空にする
```

参照：
[http://jimiprg.blog.shinobi.jp/ruby%20on%20rails/rake%E3%81%A7%E5%AE%9F%E8%A1%8C%E3%81%A7%E3%81%8D%E3%82%8Brails%E7%92%B0%E5%A2%83%E3%81%A7%E3%81%AE%E3%83%86%E3%82%B9%E3%83%88%E3%82%BF%E3%82%B9%E3%82%AF](http://jimiprg.blog.shinobi.jp/ruby%20on%20rails/rake%E3%81%A7%E5%AE%9F%E8%A1%8C%E3%81%A7%E3%81%8D%E3%82%8Brails%E7%92%B0%E5%A2%83%E3%81%A7%E3%81%AE%E3%83%86%E3%82%B9%E3%83%88%E3%82%BF%E3%82%B9%E3%82%AF)

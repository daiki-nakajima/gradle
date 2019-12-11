# 推移的依存関係の管理

> ！ソースコードは現在実行不可

[[REF]Gradle の compile, api, implementation とかについて](https://qiita.com/opengl-8080/items/6ad642e0b016465891de)

# `compile` は非推奨



# `compile` or `api` で指定した依存関係は伝播する
```
[foo] --compile--> [bar] --compile--> [commons-lang3]

  [foo] - ok -> [bar] - ok -> [commons-lang3]
    |                               ^
    |                               |
    +------------ ok ---------------+
```
`foo` プロジェクトで `bar` プロジェクトを`compile`指定し、  
`bar` プロジェクトで `commons-lang3` を `compile`指定すると、   
`foo` プロジェクトは `commons-lang3` にも依存する 

# `implementation` で指定した依存関係は伝播しない
```
[foo] --implementation--> [bar] --implementation--> [commons-lang3]

  [foo] - ok -> [bar] - ok -> [commons-lang3]
    |                               x
    |                               |
    +------------ ng ---------------+
```
`foo` プロジェクトで `bar` プロジェクトを`implementation`指定し、  
`bar` プロジェクトで `commons-lang3` を `implementation` で指定すると、  
`foo` プロジェクトは `commons-lang3` に依存しない（使用できない）

# まとめ
|`configuration` |依存関係の伝播|定義されているプラグイン|
|----------------|-------------|----------------------|
|`compile`       |する         |Java Plugin           |
|`implementation`|しない       |Java Plugin           |
|`api`           |する         |Java Library Plugin   |
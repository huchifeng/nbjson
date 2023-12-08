# nbjson

nbJSON: nano binary JSON, a minimalism binary JSON protocol

对JSON的优势, 省掉冒号,引号,逗号,转义符, 支持二进制;

不定义各种长度整型浮点型, 不关心 big/little-endian;

只有一种原子, utf8字符串或字节数组, 例如:

```
5(hello)
256(\x00...\xff)   实际占用 3长度+2括号+256=261字节
使用长度前缀, 不需要转义符, 前缀本身也是可读十进制;
```

括号用于增强可读性, 校验纠错, 代替了引号;

跟JSON一样的数组和关联数组, 例如:
```
[{4(key1)6(value1)4(key2)6(value2)}{5(bytes)3(\x01\x02\x03)}]
```

相当于json的
```
[{"key1":"value1", "key2":"value2"}, {"bytes":"\x01\x02\x03"}]
```

可以在()括号之外加空白, 增强可读性, 例如:
```
    [{ 4(key1)6(value1)  4(key2)6(value2)}
     { 5(bytes)3(\x01\x02\x03)  } ]
```

解析器还可以容错,允许在无需转义(不含\x29即')' )时省略长度前缀 :
```
[{ (key1)(value1)  (key2)(value2)}
 { (bytes)(\x01\x02\x03)  } ]
```
允许在无歧义的情况下省略长度前缀和括号, 例如:
```
[{  key1 value1  key2 value2 }
  { bytes 256(\x00...\xFF) }]
```
现有的 schema 的protobuf,msgpack比较麻烦,

schemaless 的 UBJSON BJdata 半可读, 但数据类型也很复杂, 几十种;

    

# nbjson

nbJSON: nano binary JSON, a minimalism protocol

a minimalism sample first:
```
[{ name TOM age 3 }

{ photoBytes 256(\x00... \xFF) }]
```

Eliminate colons, quotes, commas, escape characters, support binary;

NO various length integer floating types, not care about big/little-endian;

There is only one kind of atom(like LISP): utf8 string or byte array, for example:

```

5(hello)

256(\x00...\xff)  ;; actually takes 3 length +2 parentheses +256=261 bytes

```

The length prefix is used, no escape character '\\', and the prefix itself is also readable decimal format;

Parentheses are used to enhance readability and verify errors, instead of quotation marks;

Json-like arrays and associative arrays, such as:

```

[{4(key1)6(value1)4(key2)6(value2)}{5(bytes)3(\x01\x02\x03)}]

```


Json-equivalent

```

[{"key1":"value1", "key2":"value2"}, {"bytes":"\x01\x02\x03"}]

```


Can add white space outside the () bracket to improve readability, for example:

```

[{ 4(key1)6(value1) 4(key2)6(value2)}

{ 5(bytes)3(\x01\x02\x03) } ]

```


The parser can allow length prefixes to be omitted when no ambiguity:

```
[{ (key1)(value1) (key2)(value2)}

{ (bytes)(\x01\x02\x03) } ]
```

Allows the omission of length prefixes and parentheses when no ambiguity, such as:




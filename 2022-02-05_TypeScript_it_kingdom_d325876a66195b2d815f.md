<!--
title:   【TypeScript入門 #5】オブジェクトの型解釈
tags:    TypeScript,it_kingdom,オブジェクト
id:      d325876a66195b2d815f
private: false
-->

# 概要

オンラインサロン IT_KINGDOM で、typescript について学んだことを備忘録としてメモしていきます。

# 学習内容

- オブジェクトについて
- オブジェクトの表記方法

# オブジェクトについて

- オブジェクトには下記の 2 種類の意味がある
  - 下記の７種のプリミティブ型に属さないもの(正規表現、配列等）
    - 論理(boolean)型
    - 数値(number)型
    - 文字列(string)型
    - undefined 型
    - null 型
    - シンボル型(symbol)
- `{}`で表現されるもの（辞書型）

# オブジェクトの表記

表記には 4 種類があるが、下記の 2 種類が良い。Index Signature を使う方法がおすすめ。

- 標準ライブラリの`obj:Record<string, unknown> = {};`を使う方法
- Index Signature である`obj: {[key: string]: unknown} = {};`を使う方法

```Typescript:使用例
let obj1: {} = true;      // 使うべきでない(プリミティブも受けとるため)
let obj2: object = [];    // 使うべきでない(辞書型としては適してないため)
let obj3: Record<string, unknown> = {
  a: 1,
  b: "foo",
};
let obj4: {[key: string]: unknown} = {
  a: 1,
  b: "foo",
};
obj3.b = "bar";
obj4.b = "bar";
```

# 参考資料

[IT Kingdom](https://it-kingdom.com/)<br>
[サバイバル TypeScript](https://typescriptbook.jp/reference/values-types-variables/primitive-types)

<!--
title:   【typescript入門2】動的型付け・静的型付け、プリミティブ型とオブジェクト、nullとundefinedの使い分け
tags:    TypeScript,it_kingdom,動的型付け,静的型付け
id:      bb206bf2650b7e8b6a92
private: false
-->
# 概要
オンラインサロンIT_KINGDOMで、typescriptで学んだことを備忘録としてメモしていきます。

# 学習内容
- 動的型付け言語(プリミティブ型・オブジェクト型）
- 静的型付け言語

---
# 動的型付け言語
- 自動で型を付与する
- Javascriptは動的型付け言語

### プリミティブ型
- 真偽値・文字列・数値・null・undefined・BigInt・シンボル
- nullとundefinedの使い分けはやや微妙（nullは使えない変数、undefinedは初期化されていない変数を示すことが多い。typescriptではundefinedを推奨）

### オブジェクト型
- Javascriptのもつ型
- プリミティブ型以外（配列や関数など）

# 静的型付け言語
- 変数の型(文字列型、数値型等）を明示する
- 変数名：データ型で指定する

```
const foo:boolean = true;
const foo:string = "test";
const foo:number = 1;
const foo:null = null;  // nullのみ
const foo:undefined = undefined; // undefinedのみ
```

---
# 学習サイト
[オンラインサロンIT_KINGDOM](https://it-kingdom.com)
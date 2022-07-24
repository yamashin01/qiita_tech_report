<!--
title:   【TypeScript入門 #12】Index SignatureとMapped Typesの違い
tags:    TypeScript,index_signature,it_kingdom,mapped_types
id:      28e08e66f29edb83d909
private: false
-->


# 概要

オンラインサロン IT_KINGDOM で、typescript について学んだことを備忘録としてメモしていきます。

# 学習内容

- Index Signature
- Mapped Types

# Index Signature、Mapped Types とは

オブジェクトの型宣言でフィールドを指定することなくプロパティを指定できる機能

# Index Signature

- Index Signature の基本形( `key` :フィールド名。名称は任意、`K1`: key の型、`K2`: プロパティの型)<br>
  `[key:K1]: K2;`
- オブジェクトのプロパティを動的に追加する
- Index Signature を使えば、型宣言で未定義のプロパティを使うことができる

```typescript
export type User = {
  name: string;
  age: number; // index signatureでstring指定しているためエラー
  [key: string]: string; // プロパティ名はkeyでなくても良いが、慣例的にkeyがよく使われる
};

const user: User = {
  name: "taro",
  age: 20,
  account: "たろたろ", // 型宣言で[key: string]: stringと宣言されているため、プロパティaccountが明示的に宣言されてなくても使える
  job: "Software Engineer", // 同上
};
```

# Mapped Types

- Mapped Types の基本形(`P` :引数型、`K` :制約型、`T` :テンプレート型)<br>
  `[P in K]: T;`
- オブジェクトのプロパティ名を限定する
- ジェネリクスと組み合わせて型を作り出す（本稿では解説しない）

```typescript
export type User = {
  name: string;
} & PersonalData;

type PersonalData = {
  [K in "height" | "weight"]: number;
};

const user: User = {
  name: "yama",
  height: 174,
  weight: 60,
};
```

# 参考サイト

[サバイバル TypeScript](https://typescriptbook.jp/reference/values-types-variables/object/index-signature)<br>
[Mapped Types のあれこれ](https://zenn.dev/qnighy/articles/dde3d980b5e386)
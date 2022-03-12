<!--
title:   【TypeScript入門 #6】Intersection Types（交差型）、Union Types（共用型・合併型）
tags:    TypeScript,it_kingdom
id:      0e2211491869df01f6d7
private: false
-->

# 概要

オンラインサロン IT_KINGDOM で、typescript について学んだことを備忘録としてメモしていきます。

# 学習内容

- Intersection Types（交差型）
- Union Types（共用体型・合併型）

# Intersection Types（交差型）

- 複数の型を結合する
- オブジェクトなどで用いられることが多い
- プリミティブ型（number, string）などでも構文上は使えるが、意味不明となるため実務上で使うことはない

```typescript
// 結合された型では、すべての要素を保持する必要がある
type Foo = {
  a: number;
  b: string;
};
type Bar = {
  c: boolean;
};
type FooBar = Foo & Bar; // 2つの型の結合 Intersection Types

// 型Fooの要素a, b、型Barの要素cをすべて保持しているためOK
const Test: FooBar = {
  a: 2,
  b: "test",
  c: false,
};

// 型Fooの要素bを保持していないためエラー
const Test2: FooBar = {
  a: 2,
  c: false,
};
```

# Union Types（共用体型・合併型）

- 複数の型のいずれかが成立すれば OK
- Intersection Types と違い、プリミティブ型や Literal Types でも使われる
- `if`文などで型の識別がされていれば、識別された型が適用される

```typescript
// Union Types
// 結合した片方の型が成立すればOK
type Foo = {
  a: number;
  b: string;
};
type Bar = {
  a: string;
  c: boolean;
};
type FooBar = Foo | Bar; // Union Types

// Barの型を満たすためOK
const test: FooBar = {
  a: "1",
  b: "",
  c: true,
};
// Fooの型を満たすためOK
const test2: FooBar = {
  a: 1,
  b: "1",
};

if ("c" in test2) {
  // test2のkeyに'c'が存在するので、type2の型はBarとなる
  test2.a = "11";
} else {
  // test2のkeyに'c'が存在しないので、type2の型はFooとなる
  test2.b = "a";
}

if ("b" in test) {
  test.a = 1;
}
```

# 参考資料

[IT Kingdom](https://it-kingdom.com/)<br>
[Typescript 公式 HP Everyday Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types)

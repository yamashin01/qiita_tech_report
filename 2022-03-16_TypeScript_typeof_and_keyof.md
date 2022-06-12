<!--
title:   【TypeScript入門 #10】typeofとkeyofについて
tags:    TypeScript,typeof, keyof,it_kingdom
private: false
-->

# 概要

オンラインサロン IT_KINGDOM で、typescript について学んだことを備忘録としてメモしていきます。

# 学習内容

- typeof とは
- keyof とは
- typeof と keyof の組み合わせ

# typeof とは

- 型クエリ
- 指定した変数の型を取得する
- 型アノテーション（型を明示的に指定した変数）、型推論（型を指定してない変数からの型の類推）のどちらでも使える
- リテラルタイプで宣言すると型ではなく値が入る

```typescript
// 型アノテーションを取得
export let foo1: number;
type Foo1 = typeof foo1; // Foo1にはnumber型が入る

// 型推論で取得
export let foo2 = 123;
type Foo2 = typeof foo2; // Foo2にはnumber型が入る

// リテラルタイプで宣言
export const foo3 = "123";
type Foo3 = typeof foo3; // Foo3には文字列"123"が入る
```

- オブジェクトのプロパティを継承したい時にも使える

```typescript
// ユースケース1（オブジェクトの型の継承）
export const obj1 = {
  foo: "foo",
  bar: "bar",
};
const obj2: typeof obj1 = {
  foo: "aaaa",
  bar2: "bbbb", // bar2はobj1のプロパティとして定義されてないためエラー
};

// ユースケース2（型の絞り込み）
export function double(x: number | string) {
  if (typeof x === "string") {
    return Number(x) * 2; // xは必ずstring型
  }
  return x * 2; // xは必ずnumber型
}
```

# keyof とは

- 型クエリ
- オブジェクトのプロパティを取得する
- オブジェクトのプロパティ名をリテラルタイプでかつユニオンタイプで取得できる

```typescript
// リテラルタイプとユニオンタイプ
export type Obj = {
  foo: string;
  bar: number;
};

type Key = keyof Obj; // type Key = "foo" | "bar"; と同じ意味
const key1: Key = "bar"; // OK
const key2: Key = "bar2"; // Objにbar2のプロパティが定義されてないためエラー
```

# typeof と keyof の組み合わせ

- keyof と typeof を組み合わせて使うこともできる

```typescript
export const Obj2 = {
  foo: "foo",
  bar: "bar",
  123: "aaa",
};

type Key3 = keyof typeof Obj2;
const key3: Key3 = "bar"; // OK
const key4: Key3 = "aaa"; // Key3にaaaのプロパティが定義されてないためエラー
```

# 参考サイト

[サバイバル TypeScript](https://typescriptbook.jp/reference/type-reuse/keyof-type-operator)<br>
[IT Kingdom](https://it-kingdom.com/)

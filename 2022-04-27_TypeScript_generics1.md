<!--
title:   【TypeScript入門 #15】Genericsの活用
tags:    Generics,TypeScript,it_kingdom
id:      e485fddb316db8a3f72a
private: false
-->


# 概要

オンラインサロン IT_KINGDOM で、typescript について学んだことを備忘録としてメモしていきます。

# 学習内容

- [Generics とは](#genericsとは)
- [初期値の設定](#初期値の設定)
- [extends による型の制約](#extends-による型の制約)
- [関数の Generics による型推論](#関数の-generics-による型推論)
- [Generics における複数の型引数](#generics-における複数の型引数)
- [LookupTypes（ルックアップ型）](#lookuptypesルックアップ型)
- [javascript メソッドでの Generics の活用](#javascript-メソッドでの-generics-の活用)

# Generics とは

Generics とは、あらかじめ抽象的な型指定を行うこと<br>
最初の型指定を`<T>`とすることで、後で`T`に任意の型を指定できる(`T`は任意の文字列で OK)

```typescript
// Genericsの型としてTを宣言し、valueの型にTを指定
export type Foo<T> = {
  value: T;
};
const foo: Foo<number> = {
  // fooでは、Tの型としてnumber型を指定
  value: 3,
};
const foo2: Foo<string> = {
  // foo2では、Tの型としてstring型を指定
  value: "",
};
const foo3: Foo<number[]> = {
  // foo3では、Tの型としてnumber[]型を指定
  value: [1, 2, 3],
};
```

```typescript
// UserオブジェクトのプロパティstateをGenericsにする
export type User<T> = {
  name: string;
  state: T;
};

// Japanese型ではstateを東京都 or 大阪府、American型ではCA or NYに制約
type Japanese = User<"東京都" | "大阪府">;
type American = User<"CA" | "NY">;

const user1: Japanese = {
  name: "田中",
  state: "東京都",
};
const user2: American = {
  name: "Jonny",
  state: "CA",
};
```

# 初期値の設定

Generics では`<T>`の初期値となる型を設定できる。<br>
初期値を設定した場合、型宣言時に`<T>`の具体的な型を指定しなければ、初期値が入る。

```typescript
// <T>の初期値をstring型に設定
export type Foo<T = string> = {
  value: T;
};
// <T>の型を指定してないため、valueはstring型になる
const foo1: Foo = {
  value: "",
};
// <T>の型をnumber型としたため、valueはnumber型となる
const foo2: Foo<number> = {
  value: 111,
};
```

# extends による型の制約

`<T>`に`extends + 型`を追加することで、`<T>`は`extends`で指定されたインターフェースを満たす型のみ取れるようになる。

```typescript
// Tの型をstring型で制約する
export type Foo<T extends string> = {
  value: T;
};
// string型なのでOK
const foo1: Foo<string> = {
  value: "",
};
// number型はstring型を満たさないためエラー
const foo2: Foo<number> = {
  value: 111,
};
// "bar"はstring型を満たすためOK
const foo3: Foo<"bar"> = {
  value: "bar",
};
```

また、`extends`を使った型制約時も、型の初期値を指定できる

```typescript
// extendsの初期値の指定
// Tの型をstring型 or number型に制約し、初期値をstring型に設定
export type Foo<T extends string | number = string> = {
  value: T;
};
// 型の初期値はstringを指定されているため、valueはstring型になる
const foo1: Foo = {
  value: "",
};
// numberのためOK
const foo2: Foo<number> = {
  value: 111,
};
```

`extends`で複数の型に制約した際、特定の型のみに依存する機能を用いるとエラーになる。<br>
特定の型に依存する機能を用いたい場合は、型の絞り込みを行う。

```typescript
function foo<T extends string | number>(arg: T) {
  // toUpperCase()はstringのメソッドなので、stringに絞り込みを行う
  if (typeof arg === "string") {
    return { value: arg.toUpperCase() }; // stringに絞り込みが行われているためOK
  }

  return { value: arg.toUpperCase() }; // argの型がnumberの可能性があるためエラー
}
```

# 関数の Generics による型推論

関数の Generics では、関数宣言時に型が明示されていない場合、暗黙的に型推論される

```typescript
// 暗黙的な型推論
// 引数argの型をGenericsで設定
function foo<T>(arg: T) {
  return { value: arg };
}

// 関数のGenericsの型を推論する
const foo1 = foo<string | null>(""); // 引数の型はstring または nullとなる
const foo2 = foo(1); // 引数の型は暗黙的にnumberとなる
const foo3 = foo([false]); // 引数の型は暗黙的に[boolean]になる
```

# Generics における複数の型引数

Generics の型は、複数指定することができる。(例：`<T, K, U>`)

```typescript
// Genericsの型引数を複数持つ
const foo = <T, K, U>(foo: T, bar: K, baz: U) => {
  return {};
};

// 各々の型に対してextendsによる制約をかけることもできる
const foo2 = <T extends string, K extends number, U = boolean>(
  foo: T,
  bar: K,
  baz: U
) => {
  return {};
};
```

# LookupTypes（ルックアップ型）

オブジェクトのプロパティ(key)に対応する値(value)の型にアクセスする型を Lookup Types（ルックアップ型）という。<br>
オブジェクトのプロパティを Generics で指定する場合、`extends keyof`による型の制約が必要となる。

```typescript
type Obj = {
  a: number;
  b: boolean;
};

type Foo = Obj["a"]; // Fooの型はnumberになる

const getProperty = <T, K>(obj: T, key: K) => {
  return obj[key]; // 型Kが型Tのプロパティ(key)として設定されてないため、エラー
};
const getProperty2 = <T, K extends keyof T>(obj: T, key: K) => {
  return obj[key]; // 型Kが型Tのプロパティ(key)として設定されているため、OK
};

const setProperty = <T, K extends keyof T>(obj: T, key: K, value: T[K]) => {
  obj[key] = value;
};
const obj = {
  foo: 1,
  bar: 2,
  baz: 3,
};
setProperty(obj, "bar", 100); // obj[bar]の型はnumber型のため、100はOK
setProperty(obj, "bar", "bar2"); // obj[bar]の型はnumber型のため、"bar2"はエラー
```

# javascript メソッドでの Generics の活用

javascript のメソッド(`map()`など)でも暗黙的に Generics が使われている。

```typescript
const foo = [1, 2, 3].map((v) => v); // fooの型はnumber[]になる。
const foo2 = [1, 2, 3].map((v) => v.toString()); // foo2の型はstring[]になる。
```

# 参考サイト

[【TypeScript】Generics(ジェネリックス)を理解する](https://qiita.com/k-penguin-sato/items/9baa959e8919157afcd4)<br>
[サバイバル TypeScript](https://typescriptbook.jp/reference/generics)<br>
[駆け足エンジニアの記録](https://tsuboi99553758.hatenablog.com/entry/2020/09/14/105123)<br>
[IT Kingdom](https://it-kingdom.com/)
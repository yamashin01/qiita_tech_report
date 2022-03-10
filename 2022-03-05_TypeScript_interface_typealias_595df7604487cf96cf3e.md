<!--
title:   【TypeScript入門 #7】InterfaceとTypeAliasの違い
tags:    TypeScript,interface,typealias
id:      595df7604487cf96cf3e
private: false
-->
# 概要

オンラインサロンIT_KINGDOMで、typescriptについて学んだことを備忘録としてメモしていきます。

# 学習内容
- Type Alias, Interfaceとは
- Type AliasとInterfaceの違い

# type alias, Interfaceとは
どちらも型の定義をするもの。

```typescript:Type Alias
type Foo = {
  a: number;
};

const foo: Foo = {
  a: 1,
};
```
```typescript:Interface
interface Foo {
  a: number;
}
const foo: Foo = {
  a: 1,
};

```

# Type AliasとInterfaceの違い

- 宣言できる型にちがいがある
 Interfaceでは宣言できるのはオブジェクトのみ。
Type Aliasではどんな型でも宣言できる。

- open-endedに準拠しているかどうか
interfaceでは同じ名前の型宣言をすることが可能。

```
// interfaceでは、open-endedに準拠するため、Fooを2回宣言してもエラーにならない（Type Aliasではエラーになる）
interface Foo {
  a: number;
}
interface Foo {
  b: number;
}
const foo: Foo = {
  a: 1,
  b: 2,
}
```

- 継承できるかどうか
Interfaceでは、`extends`を使って型の継承をすることができる。
Type Aliasで同じ機能を実現するには、`&`によるIntersection Typesを使う必要がある。

```typescript: interfaceによる継承
interface Foo {
  a: number;
}
interface Bar extends Foo {
  b: number;
}
const foo: Bar = {
  a: 1,
  b: 2,
};
````

```typescript: Type AliasによるIntersection Types
Type Foo = {
  a: number;
}
Type Bar =  Foo & {
  b: number;
}
const foo4: Bar = {
  a: 1,
  b: 2,
};
````

- プロパティのオーバーライドが可能か？
Type AliasのIntersection Typesでは、プロパティのオーバーライドが可能。
Interfaceの継承では、プロパティをオーバーライドするとエラーになる。

```typescript:Type Aliasでのプロパティaのオーバーライド
type Foo = {
  a: number;
}
// ここではエラーにならない
type Bar = Foo & {
  a: string;
}
const foo: Bar = {
  a: 1,
}
```

```typescript: Interfaceによるオーバーライド
interface Foo {
  a: number;
}
// ここでエラーになる
interface Bar extends Foo {
  a: string;
}
const foo: Bar = {
  a: 1,
}
```

- Mapped Typesが使えるかどうか
Type Aliasでは、Mapped Typesを使うことができる。

```typescript:Type AliasによるMapped Typesの使用例
type Animals = "dog" | "cat";

type Foo7 = {
  [key in Animals] : number;
}
const foo7: Foo7 = {
  dog:1,
  cat:2,
}
```

# 結論：Type AliasとInterfaceのどちらを使うと良いか
どちらを使っても良い（公式ドキュメントでもどちらでも良いと紹介されているため）。
どちらかといえばType Aliasがおすすめ。

# 参考サイト
[typescript documentation](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)
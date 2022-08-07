<!--
title:   【TypeScript入門 #16】Condition TypesとInfer
tags:    TypeScript,conditionTypes Infer,it_kingdom
private: false
-->

# 概要

オンラインサロン IT_KINGDOM で、typescript について学んだことを備忘録としてメモしていきます。

# 学習内容

- [Conditional Types とは](#conditional-types-とは)
- [Infer とは](#infer-とは)

# Conditional Types とは

Typescript2.8 で導入された、条件分岐による型推論を行う機能。普段現場で使われることはあまりない様子。<br>
下記の`T[K] extends string ? K : never`部分が Conditional Types。<br>
下記の例では、三項演算子により、`T[K]`の型が string 型のサブタイプかどうかで`ConditonalType`の型が変わる。

```typescript
type ConditionalType<T> = {
  [K in keyof T]: T[K] extends string ? K : never;
}[keyof T];
```

```typescript
type Props = {
  id: string;
  name: string;
  age: number;
};

// T[K] extends string ? K : never 部分がConditional Types
// [keyof T]をつけることで、Lookup Typesとなる。
type FilterString<T> = {
  [K in keyof T]: T[K] extends string ? K : never;
}[keyof T];

// StringKeysの要素は、string型のidとnameになる(number型のageは除外される)。
type StringKeys = FilterString<Props>;

type Filter<T, U> = {
  [K in keyof T]: T[K] extends U ? K : never;
}[keyof T];

type StringKeys2 = Filter<Props, string>; // string型の要素、id、name
type NumberKeys2 = Filter<Props, number>; // number型の要素、age
```

# Infer とは

部分的な型を取り出す機能。

```typescript
// 関数の型を部分的に取り出す
const foo = () => {
  return true;
};

type Return<T> = T extends () => infer U ? U : never;

type Foo = Return<typeof foo>; // Fooの型はfooの戻り値の型、booleanになる

const foo2 = "";
type Foo2 = Return<typeof foo2>; // fooは関数ではないため、Foo2の型はneverになる

// 引数の型を部分的に取り出す
const foo3 = (id: string) => {
  return 3;
};
type Args<T> = T extends (id: infer U) => any ? U : never;
type Foo3 = Args<typeof foo3>; // Foo3の型はfoo3の引数の型、stringになる

// 可変長引数の型を取り出す
const foo4 = (id: string, name: string) => {
  return 0;
};
type Args2<T> = T extends (...args: infer U) => any ? U : never;
type Foo4 = Args2<typeof foo4>; // Foo4の要素はfoo4の引数、id:stringとname:stringになる

type Args3<T> = T extends (...args: [any, infer U, ...any]) => any ? U : never;
type Foo5 = Args3<typeof foo4>; // Foo5の要素はfoo4の2番目の引数、name:stringになる
```

# 参考サイト

[IT Kingdom](https://it-kingdom.com/)
[【TypeScript】Conditional Type で型の条件分岐を行う](https://zenn.dev/oreo2990/articles/1040312d7af066)

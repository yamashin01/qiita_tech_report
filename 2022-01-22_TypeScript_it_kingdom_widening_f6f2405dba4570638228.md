<!--
title:   【typescript入門3】Literal Types・Wideningの概念
tags:    TypeScript,it_kingdom,widening,リテラル型
id:      f6f2405dba4570638228
private: false
-->

# 概要

オンラインサロン IT_KINGDOM で、typescript で学んだことを備忘録としてメモしていきます。

# 学習内容

- リテラル型(Literal Types)
- Widening

# リテラル型(Literal Types)

- Boolean, String, Number で使用
- 変数に入力する値を指定の値のみに固定する

```typescript
// 使用例1
const isTrue: true = true; // boolean literal types： isTrueにtrue以外の値を入れるとエラー
const foo: "foo" = "foo"; // string literal types ： fooに"foo"以外の値を入れるとエラー
const numTwo: 2 = 2; // number literal types ： numTwoに2以外の値を入れるとエラー
const direction: "North" | "South" | "West" | "East" = "North"; // 'North', 'South', 'West', 'East'以外が入るとエラー
```

```typescript
// 使用例2
const Home: NextPage = () => {
  return <div><Component foo /></div>;
}

const Component = (props : { foo?: true}) => {
  if (props.foo) {
    return <div>a</div>;
  }
```

# Widening

- 型が自動で拡張する機能
  - `const`で宣言した Literal Types の変数を別の変数に代入すると、通常の型(string, number 等)で渡される
  - `let`で宣言した Literal Types の変数では、Widening は発生しない
- Widening を防ぐために、`as const`等を用いる。

```typescript
// 例1 : constでLiteral Typesの型を宣言(Widening発生)
const foo: "foo" = "foo"; // Literal Typesの型を宣言
let bar = foo; // 変数fooを変数barに代入
bar = "bar"; // WideningによりErrorにならない
```

```typescript
// 例2 : letでLiteral Typesの型を宣言(Wideningが発生しない)
let foo: "foo" = "foo"; // Literal Typesの型を宣言
let bar = foo; // 変数fooを変数barに代入
bar = "bar"; // 変数barの値は"foo"で固定されているため、Errorになる
```

```typescript
// 例3 : as constでLiteral Typesの型を宣言(Wideningが発生しない)
const foo = "foo" as const; // as constで変数fooのLiteral Typesの型を宣言
let bar = foo; // 変数fooを変数barに代入
bar = "bar"; // 変数barの値は"foo"で固定されているため、Errorになる
```

# 参考サイト

[typescript 公式サイト](https://typescript-jp.gitbook.io/deep-dive/type-system/literal-types)<br>
[オンラインサロン IT_KINGDOM](https://it-kingdom.com)

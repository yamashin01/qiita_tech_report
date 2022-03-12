<!--
title:   【TypeScript入門 #4】TypeScriptの色んな型の紹介
tags:    TypeScript,it_kingdom
id:      1a0253b29e45335d7f30
private: false
-->

# 概要

オンラインサロン IT_KINGDOM で、typescript について学んだことを備忘録としてメモしていきます。

# 学習内容

- 6 つの型の紹介 Array, Tuple, Any, Unknown, Void, Never

## Array 型

- 配列の型を指定できる
- 書き方には`型名[]`と`Array<型名>`の 2 種類がある（`型名[]`の方が一般的らしい）
- 複数の型指定も可能

```typescript
const foo: number[] = [1, 2]; // 型名[]
const foo: Array<number> = [1, 2]; // Array<型名>
const foo: (number | string | boolean)[] = ["a", 1, true]; // 複数の型指定
```

## Tuple 型(タプル型)

- 要素の数や各要素の型を指定することができる
- 要素のメソッドを補完できる
- Tuple 型を使える場合は、Array 型ではなく Tuple 型を使う方が良い

```typescript
const foo: [string, number] = ["fooo", 0.00635]; // Tuple型で配列を宣言
foo[1].toFixed(2); // 配列の2番目の値(0.00635)をメソッドで整形
```

## Any 型

- 型指定しない(基本使わない方が良い)
- 初期に型指定しづらい場合に使う

```typescript
let foo: any = true;
foo = 5; // 別の型を代入してもエラーにならない
```

## Unknown 型

- 型不明の場合に使う（Any 型と近い）
- Any と違って、メソッド等を制御できる

```typescript
const foo: unknown = "foo"; // unknownで型指定
foo.substr(2); // substr()はstring型のメソッドのため、エラーになる
if (typeof foo === "string") {
  foo.substr(2); // if文でstring型に制御しているため、エラーにならない
}
```

## Void 型

- 関数の戻り値がないことを示す

```typescript
const foo = (): void => {
  alert("hello");
  return; // 関数の戻り値なし
};
const foo: () => void = () => {
  alert("hello");
};
```

## Never 型

- 発生しない場合の型に付与

```typescript
const foo = (bar: "a" | "b") => {
  switch (bar) {
    case "a":
      return;
    case "b":
      bar;
      return;
    default:
      bar; // never型（決してここには来ないため）
      break;
  }
};
```

# 参考サイト

[オンラインサロン IT KINGDOM](https://it-kingdom.com/)

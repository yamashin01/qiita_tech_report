<!--
title:   【TypeScript入門 #4】TypeScriptの色んな型の紹介
tags:    TypeScript,it_kingdom
id:      1a0253b29e45335d7f30
private: false
-->
# 概要
オンラインサロンIT_KINGDOMで、typescriptについて学んだことを備忘録としてメモしていきます。

---
# 学習内容
- 6つの型の紹介 Array, Tuple, Any, Unknown, Void, Never

---
## Array型
-  配列の型を指定できる
- 書き方には`型名[]`と`Array<型名>`の2種類がある（`型名[]`の方が一般的らしい）
- 複数の型指定も可能

```
const foo: number[] = [1, 2];                                // 型名[]
const foo: Array<number> = [1, 2];                           // Array<型名>
const foo: (number | string | boolean)[] = ["a", 1, true];   // 複数の型指定
```

## Tuple型(タプル型)
- 要素の数や各要素の型を指定することができる
- 要素のメソッドを補完できる
- Tuple型を使える場合は、Array型ではなくTuple型を使う方が良い

```
const foo : [string, number] = ["fooo", 0.00635];   // Tuple型で配列を宣言
foo[1].toFixed(2);                                  // 配列の2番目の値(0.00635)をメソッドで整形
```

## Any型
- 型指定しない(基本使わない方が良い)
- 初期に型指定しづらい場合に使う

```
let foo: any = true;
foo = 5;              // 別の型を代入してもエラーにならない
```

## Unknown型
- 型不明の場合に使う（Any型と近い）
- Anyと違って、メソッド等を制御できる

```
const foo: unknown = "foo";      // unknownで型指定
foo.substr(2);                   // substr()はstring型のメソッドのため、エラーになる
if (typeof foo === "string") {
  foo.substr(2);                 // if文でstring型に制御しているため、エラーにならない
}
```

## Void型
- 関数の戻り値がないことを示す

```
const foo = (): void => {
  alert("hello");
  return;                 // 関数の戻り値なし
}
const foo : () => void = () => {
  alert("hello");
}
```

## Never型
- 発生しない場合の型に付与

```
const foo = (bar: "a" | "b") => {
  switch (bar) {
    case "a":
      return;
    case "b":
      bar;
      return;
    default:
      bar;  // never型（決してここには来ないため）
      break;
  }
}
```

# 参考サイト
[オンラインサロンIT KINGDOM](https://it-kingdom.com/)
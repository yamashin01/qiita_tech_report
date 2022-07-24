<!--
title:   【TypeScript入門 #11】ダウンキャストとアップキャストについて
tags:    TypeScript,downcast,it_kingdom,upcast
id:      63e283708c0d35815357
private: false
-->


# 概要

オンラインサロン IT_KINGDOM で、typescript について学んだことを備忘録としてメモしていきます。

# 学習内容

- ダウンキャスト とは
- アップキャスト とは
- Non-null アサーション
- Double アサーション

# ダウンキャスト とは

- 一度定義した型を厳密化すること
- 基本系は`as XXXX`(XXXX は定義したい型)または`as const`
- `as const`をつける方法を const アサーションという

```typescript
// オブジェクトのプロパティの型はstringとなる（JavaScriptの仕様）
const theme1 = {
  color: "red",
};
theme.color = "blue"; // theme.colorの型はstringなのでエラーとならない

// as "red"をつけると、colorの型は"red"として定義される
const theme2 = {
  color: "red" as "red",
};
theme2.color = "blue"; // theme2.colorの型は"red"なのでエラーになる
```

```typescript
// constアサーションの記法1
const theme1 = {
  color: "red" as const,
  flag: true,
};
theme1.color = "blue"; // theme1.colorの型は"red"でconstアサーションされているため、エラーとなる
theme1.flag = false; // theme1.flagの型はconstアサーションされてないため、エラーとならない

// constアサーションの記法2
const theme2 = {
  color: "red",
  backGroundColor: "blue",
} as const;
theme2.color = "blue"; // themeオブジェクトの型を厳密化されたのでエラーになる
theme2.backGroundColor = "green"; // themeオブジェクトの型を厳密化されたのでエラーになる
```

ダウンキャストは widening の防止にも役立つ<br>
[widening の詳細](https://qiita.com/yamashin0616/items/f6f2405dba4570638228)

```typescript
// wideningにより、xの型は"red"ではなくstringとなる
export const color = "red"; // colorの型は"red"と厳密に定義
let x = color; // xの型はstringとなる

// constアサーションによるwideningの防止
export const color2 = "red" as const;
let x2 = color2; // xの型は"red"となる
```

# アップキャスト とは

- 一度定義した型を抽象化すること
- 型の抽象化。基本的には使うべきではない

```typescript
export const color = "red" as const;
let x = color as any; // xの型はanyになる。
```

# Non-null アサーション（非推奨）

- `str!`とすることで`str`が non-null であることを示す
- 読み取れない型をとりうる場合にエラーとする typescript の設定をオフにする
- あまり使わない方が良い

```typescript
export function getFirstLetter(str?: string) {
  return str.charAt(0); // strがstringでないケースがあるのでエラーとなる
}
```

```typescript
export function getFirstLetter2(str?: string) {
  return str!.charAt(0); // strがundefinedの可能性を消し去る処理する
}
```

# Double アサーション（非推奨）

- 一度型指定された変数に、`as unknown`や`as any`をつけることで別の型をつける記法
- あまり使わない方が良い
- 使うシーンとしては、外部パッケージが間違っている時など

```typescript
export function getFirstLetter3(str: number) {
  // return (str as string).charAt(0);
  return (str as unknown as string).charAt(0);
}
```

# 参考サイト

[TypeScript のコードレビューを依頼された人のための!と?の解説](https://dev.classmethod.jp/articles/typescript-assertions/)<br>
[TypeScript Deep Dive](https://typescript-jp.gitbook.io/deep-dive/type-system/type-assertion)<br>
[IT Kingdom](https://it-kingdom.com/)
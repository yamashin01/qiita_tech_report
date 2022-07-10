<!--
title:   【TypeScript入門 #14】ユーザー定義の型ガード(TypeGuard)について
tags:    TypeScript,it_kingdom
private: false
-->

# 概要

オンラインサロン IT_KINGDOM で、typescript について学んだことを備忘録としてメモしていきます。

# 学習内容

- ユーザー定義の型ガードとは
- ユーザー定義の型ガードの使用例
  - 非同期処理の型ガード
  - filter 関数の型ガード

# ユーザー定義の型ガードとは

の型が不明(any)の場合、通常の型ガードでは型の絞り込みはできない。

```typescript
type UserA = { name: string; lang: "ja" };
type UserB = { name: string; lang: "en" };

export const foo = (value: any) => {
  if (value.lang === "ja") {
    // valueの型が不明(any)の場合、型の絞り込みを行うことができない
    return value; // 型:any
  }
  return value; // 型:any
};
```

ユーザー定義の型ガードを定義することにより、上記の場合も型の絞り込みができる。

```typescript
// `user is UserA`は、本関数のuserがUserAを必ず返すことを示す
const isUserA = (user: UserA | UserB): user is UserA => {
  return user.lang === "ja";
};
// `user is UserB`は、本関数のuserがUserBを必ず返すことを示す
const isUserB = (user: UserA | UserB): user is UserB => {
  return user.lang === "en";
};

export const foo2 = (value: any) => {
  if (isUserA(value)) {
    // valueの型が不明(any)の場合でも、返り値の型をUserAに絞り込める
    return value; // 型: UserA
  }
  if (isUserB(value)) {
    // valueの型が不明(any)の場合でも、返り値の型をUserBに絞り込める
    return value; // 型: UserB
  }
  return value;
};
```

# ユーザー定義の型ガードの使用例

## 非同期処理の型ガード

通常、API 等で取得した値の型は any になる。<br>
ユーザー定義の型ガードにより、型を付与することができる。

```typescript
type UserA = { name: string; lang: "ja" };
type UserB = { name: string; lang: "en" };

const isUserA = (user: UserA | UserB): user is UserA => {
  return user.lang === "ja";
};
const isUserB = (user: UserA | UserB): user is UserB => {
  return user.lang === "en";
};

// APIで取得した変数の型はanyになる
export const foo = async () => {
  const res = await fetch("");
  const json = await res.json();
  if (isUserA(json)) {
    // `isUserA`で絞り込むことにより、`json`の型をuserAに絞り込むことができる
    return json.lang;
  }
};
```

## filter 関数の型ガード

現状、filter 関数の返り値に対して型の絞り込みはされていない（Typescript のバージョンアップにより改善される可能性あり）。<br>
ユーザー定義の型ガードにより、filter 関数の返り値の型を絞り込む。

```typescript
type UserA = { name: string; lang: "ja" };
type UserB = { name: string; lang: "en" };

const isUserA = (user: UserA | UserB): user is UserA => {
  return user.lang === "ja";
};
const isUserB = (user: UserA | UserB): user is UserB => {
  return user.lang === "en";
};

const users: (UserA | UserB)[] = [
  { name: "たなか", lang: "ja" },
  { name: "やまだ", lang: "ja" },
  { name: "ジョニー", lang: "en" },
];

// 下記のfilterでは、japaneseの型をuserAには絞り込めていない
const japanese = users.filter((user) => {
  return user.lang === "ja"; // 型: (UserA | UserB)[]
});
// `isUserA`を用いることでjapaneseの型をuserAに絞り込める
const japanese2 = users.filter(isUserA); // 型: UserA[]
```

# 参考

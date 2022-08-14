<!--
title:   【TypeScript入門 #17】Utility Typesとは
tags:    TypeScript,utilityTypes,it_kingdom
private: false
-->

# 概要

オンラインサロン IT_KINGDOM で、typescript について学んだことを備忘録としてメモしていきます。

# 学習内容

- [UtilityTypes とは](#utilitytypes-とは)
- [Readonly](#readonly)
- [Partial](#partial)
- [Required](#required)
- [Pick](#pick)
- [Omit](#omit)
- [Extract](#extract)
- [Exclude](#exclude)
- [NonNullable](#nonnullable)
- [Record](#record)
- [Parameters](#parameters)
- [Upercase / Lowercse / Capitalize / Uncapitalize](#upercase--lowercse--capitalize--uncapitalize)
- [type-fest](#type-fest)

# UtilityTypes とは

既存で準備されている、便利な型のライブラリ。Typescript が公式に準備している型と非公式な型がある。<br>
ここでは、よく使われる型をいくつか紹介する。

# Readonly

オブジェクトの要素を読み取り専用にする。

```typescript
// User型では、nameとageが必須項目、countryを任意項目とする
type User = {
  name: string;
  age: number | null;
  country?: "US" | "UK" | "JP";
};

// Readonly型により、ReadOnlyUserの各要素は読み取り専用になる
type ReadOnlyUser = Readonly<User>;
const user: ReadOnlyUser = {
  name: "taro",
  age: 20,
};
user.age = 30; // ageは読み取り専用のため、エラーになる
```

# Partial

オブジェクトの全ての要素をオプショナル指定（任意指定）にする

```typescript
// User型では、nameとageが必須要素、countryをオプショナル要素とする
type User = {
  name: string;
  age: number | null;
  country?: "US" | "UK" | "JP";
};

// ageが未設定のため、User型のuser1はエラーになる
const user1: User = {
  name: "taro",
};
// PartialUser型ではUserの全ての要素がオプショナル要素となるため、user2はOK
type PartialUser = Partial<User>;
const user2: PartialUser = {
  name: "taro",
};
```

# Required

オブジェクトの全ての要素を必須にする

```typescript
// User型では、nameとageが必須要素、countryをオプショナル要素とする
type User = {
  name: string;
  age: number | null;
  country?: "US" | "UK" | "JP";
};

// User型ではcountry要素をオプショナルとしているため、user1はOK
const user1: User = {
  name: "taro",
  age: 20,
};
// RequiredUser型ではcountry要素も必須となるため、user2はエラーとなる
type RequiredUser = Required<User>;
const user2: RequiredUser = {
  name: "taro",
  age: 20,
};
```

# Pick

あるオブジェクトから、特定の要素のみを抜き出して新たなオブジェクトを生成する

```typescript
// User型では、nameとageが必須要素、countryをオプショナル要素とする
type User = {
  name: string;
  age: number | null;
  country?: "US" | "UK" | "JP";
};

// PickUser型では要素は"name"と"country"のみとなるため、age:20はエラーとなる
type PickUser = Pick<User, "name" | "country">;
const user: PickUser = {
  name: "taro",
  age: 20,
  country: "JP",
};
```

# Omit

あるオブジェクトから、特定の要素のみを外して新たなオブジェクトを生成する

```typescript
// User型では、nameとageが必須要素、countryをオプショナル要素とする
type User = {
  name: string;
  age: number | null;
  country?: "US" | "UK" | "JP";
};

// OmitUser型ではage要素が外されるため、age:20はエラーとなる
type OmitUser = Omit<User, "age">;
const user: OmitUser = {
  name: "taro",
  age: 20,
  country: "JP",
};
```

# Extract

2 つの型引数のうち、2 つ目の型引数と互換性のある型を 1 つ目の型引数から取り出す

```typescript
type FooA = Extract<100, number>; // FooAの型は100（100はnumber型の1つのため）
type FooB = Extract<number, 100>; // FooBの型はnever（number型の100に含まれないため）
type Foo1 = Extract<string | number, string>; // Foo1の型はstring
type Foo2 = Extract<"hello" | 0, string>; // Foo2の型は"hello"
type Foo3 = Extract<string | number, "hello" & boolean>; // Foo3の型はnever（互換性のある型がないため）
```

# Exclude

2 つの型引数のうち、2 つ目の型引数と互換性のない型を 1 つ目の型引数から取り出す

```typescript
type FooA = Exclude<"hello", number>; // Fooの型は"hello"（"hello"はnumber型と互換性がないため）
type FooB = Exclude<number, "hello">; // Fooの型はnumber（number型は"hello"と互換性がないため）
type Foo1 = Exclude<string | number, string>; // Foo1の型はnumber
type Foo2 = Exclude<"hello" | 0, string>; // Foo2の型は0
type Foo3 = Exclude<string | number, "hello" & boolean>; // Foo3の型はstring | number
```

# NonNullable

型引数で指定した型から、null と undefined を削除する

```typescript
type Foo = NonNullable<string | null | undefined>; // Fooの型はstring
```

# Record

1 つ目の型引数でオブジェクトの key の型、2 つ目の型引数でオブジェクトの value の型を指定する

```typescript
// 型Fooのオブジェクトでは、keyの型がstring、valueの型がnumberになる
type Foo = Record<string, number>;

const obj: Foo = {
  hoge: 1, // OK
  fuga: "3", // valueの型がstringのためエラー
};
```

# Parameters

関数の引数の型を tuple として指定する

```typescript
function foo(a: string, b: number[], c: boolean) {
  return;
}

type Foo = Parameters<typeof foo>; // 関数Fooの型は、関数fooの引数の型 [string, number[], boolean]
const bar: Foo = ["a", [10, 20], true]; // 関数fooの引数の型と同じためOK
const hoge: Foo = [10, [20, 30, 40], false]; // 第一引数の型が不一致のためエラー
```

# Upercase / Lowercse / Capitalize / Uncapitalize

型となる文字列を変換する

- Uppercase : 文字列を大文字にする
- Lowercase : 文字列を小文字にする
- Capitalize : 文字列のうち先頭のみを大文字にする
- Uncapitalize: 文字列のうち先頭を小文字にする

```typescript
type Foo1 = Uppercase<"hello">; // Foo1の型は"HELLO"
type Foo2 = Lowercase<"HELLO">; // Foo2の型は"hello"
type Foo3 = Capitalize<"hello">; // Foo3の型は"Hello"
type Foo4 = Uncapitalize<"HELLO">; // Foo4の型は"hELLO"
```

# type-fest

標準の UtilityTypes ではカバーしきれない機能を実現するための型機能ライブラリ。<br>
下記コマンドで`type-fest`をインストールする必要がある。

```
npm install type-fest
or
yarn add type-fest
```

ここでは、`type-fest`の一つである PartialDeep（ネスト構造の型まで全てオプショナル指定にする機能）について紹介する。<br>
他の機能は、[Github type-fest](https://github.com/sindresorhus/type-fest)から参照できる。

```typescript
type User = {
  name: string;
  age: number | null;
  address: {
    country: "US" | "UK" | "JP"; // ネスト構造
  };
};

// Partialの場合
type PartialUser = Partial<User>;
const user: PartialUser = {
  name: "太郎",
  address: {}, // User内のネスト構造にあるcountryがオプショナル指定できていないため、countryの要素がない書き方ではエラーになる
};

// PartialDeepの場合
import type { PartialDeep } from "type-fest";

type PartialDeepUser = PartialDeep<User>;
const user2: PartialDeepUser = {
  name: "太郎",
  address: {}, // User内のネスト構造にあるcountryもオプショナル指定されるため、countryようがなくてもOK
};
```

# 参考サイト

[IT Kingdom](https://it-kingdom.com/)
[TypeScript の便利な型コレクション type-fest と型パズル解説~前編~](https://shisama.hatenablog.com/entry/2019/12/23/080000#type-fest)
[Github type-fest](https://github.com/sindresorhus/type-fest)

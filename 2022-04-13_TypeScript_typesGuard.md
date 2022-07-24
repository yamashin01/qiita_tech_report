<!--
title:   【TypeScript入門 #13】Type Guard(型ガード)
tags:    TypeScript,it_kingdom
id:      255b515e4b29aeac0b16
private: false
-->


オンラインサロン IT_KINGDOM で、typescript について学んだことを備忘録としてメモしていきます。

### 学習内容

- Type Guard とは
- typeof を使った TypeGuard
- JS メソッドを使った TypeGuard
- in 演算子を使った TypeGuard
- タグ付き UnionTypes を使った TypeGuard

### TypeGuard とは

条件式を用いて型を制限すること。型チェックを強化することで型に関する予期せぬエラーを防ぐ。

### typeof を使った TypeGuard

- `typeof`で変数の型を制限する

```typescript
export const foo = (value: string | number | boolean) => {
  if (typeof value === "string") {
    return value; // stringのみ
  } else if (typeof value === "number") {
    return value; // numberのみ
  }
  return value; // booleanのみ
};
```

### JS メソッドを使った TypeGuard

- JS で準備されている型取得機能を用いて、型を制限する（例：`isArray`）

```typescript
export const foo = (value?: string | string[]) => {
  if (Array.isArray(value)) {
    return value; // string[]のみ
  } else if (!value) {
    return value; // string または undefined
  }
  return value; // stringのみ
};
```

### in 演算子を使った TypeGuard

- 型のプロパティの型の違いを用いて、型を制限する

```typescript
type UserA = { name: string };
type UserB = { name: string; nickName: string };

export const foo = (value: UserA | UserB) => {
  if ("nickName" in value) {
    return value; // nickNameを持つUserB
  }
  return value; // nickNameを持たないUserA
};
```

### タグ付き UnionTypes を使った TypeGuard

- 型のプロパティの値を UnionTypes で識別し、型を制限する

```typescript
type UserA = { name: string; lang: "ja" };
type UserB = { name: string; lang: "en" };
type UserC = { name: string; lang: "fr" };

export const foo = (value: UserA | UserB | UserC) => {
  switch (value.lang) {
    case "ja":
      return value; // jaを持つUserA
    case "en":
      return value; // enを持つUserB
    case "fr":
      return value; // frを持つUserC
    default:
      return value; // こもこもnever
  }
};
```

### 参考

[TypeScript Deep Dive](https://typescript-jp.gitbook.io/deep-dive/type-system/typeguard)
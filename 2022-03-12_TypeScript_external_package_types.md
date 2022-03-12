<!--
title:   【TypeScript入門 #8】型宣言ファイルと外部パッケージの型利用
tags:    TypeScript,interface,typealias,it_kingdom
private: false
-->

# 概要

オンラインサロン IT_KINGDOM で、typescript について学んだことを備忘録としてメモしていきます。

---

### 学習内容

- 外部で定義された型とは
- d.ts とは

---

### 外部で定義された型とは

- コード開発する際には、通常、自分で定義した型だけでなく、外部で定義された型も用いる。
- 外部パッケージの型を見に行くためには、F12 ボタンでコードジャンプできる。

```typescript: NextPage.tsx
// Next.jsで使われるNextPageは外部で定義された型
const Home: NextPage = () => {
  return (
      <div></div>
  )
}

// NextPageの型では、undefinedは設定されていないため、下記はエラーになる。
const Home: NextPage = () => {
  return undefined;
}
```

```typescript: index.d.ts
/**
 * `Page` type, use it as a guide to create `pages`.
 */
export type NextPage<P = {}, IP = P> = NextComponentType<NextPageContext, IP, P>

```

### .d.ts とは

- 型定義ファイルのこと
- 型定義 NextPage は nodemodules/next/types/index.d.ts ファイルで指定している

# 参考サイト

[IT Kingdom](https://it-kingdom.com/)
[typescript documentation](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

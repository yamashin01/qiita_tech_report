<!--
title:   【typescript入門1】型推論・型アノテーション・型アサーション
tags:    TypeScript,it_kingdom,型アサーション,型アノテーション,型推論
id:      53f139f9e12c2e6d338c
private: false
-->

# 概要

オンラインサロン IT_KINGDOM で学んだことを、typescript で学んだことを備忘録としてメモしていきます。

# 学習内容

- 型推論
- 型アノテーション（型注釈）
- 型アサーション

# 型推論

- 型を自動で付与する

# 型アノテーション

- 開発者による型の設定

## メリット

- 開発者のドキュメントの役割
- コンパイル速度が上がる
- 型推論のみでは足りない場面がある

# 型アサーション

- 一度設定した型を上書きする(as を用いる)
- 多用すべきではない
- TypeGuard という方法で多用を防ぐ方法もある

```typescript
let foo = {} as {bar: number};

foo.bar = 1;

function double(x: number): number  | undefined {
  if (x > 0) {
    return;
  }
  return x * 2;
```

# 参考資料

[IT KINGDOM](https://it-kingdom.com/)

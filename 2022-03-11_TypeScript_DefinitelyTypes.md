<!--
title:   【TypeScript入門 #9】Definitely Typed(@types)とは
tags:    TypeScript,Types,it_kingdom
id:      3f485f7af7435564d90d
private: false
-->


# 概要

オンラインサロン IT_KINGDOM で、typescript について学んだことを備忘録としてメモしていきます。

# 学習内容

- Definitely Typed(@types)とは
- パッケージの選び方

# Next.js と React の型定義ファイル(index.d.ts）の保存場所

- Next.js の型の保存場所<br>
  node_modules > next > types > index.d.ts
- React の型の保存場所<br>
  node_modules > @types > react > index.d.ts

# 型定義ファイルの指定

config.json ファイル内の `@types/react` で指定。

```
  "devDependencies": {
    "@types/react": "17.0.32",
    "autoprefixer": "^10.4.2",
    "eslint": "8.10.0",
```

# 外部パッケージの選び方

下記の優先順位で選択すると良い

1. TypeScript 製であるパッケージ
   Github 上の `Languages` で言語を確認できる
2. JavaScript 製でも型定義ファイル(index.d.ts)がパッケージ内にあるパッケージ
   index.d.ts を人間の手で作るため、誤っている可能性がある
3. 型定義ファイルがパッケージ内にないが、DefinitelyTyped にあるもの
4. JavaScript 製で型定義ファイルが DefinitelyTyped にもないパッケージ
   自分で型定義ファイルを生成する必要があるため、初心者のうちは避ける方が良い

# 型定義ファイルの生成方法

ts ファイルまたは tsx ファイルのコンパイル時に、`-d`オプションをつける

```
npx tsc ./index.ts -d
```

# 参考サイト

[IT Kingdom](https://it-kingdom.com/)
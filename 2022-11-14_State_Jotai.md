<!--
title:   【React状態管理6】Jotai
tags:    react, state, jotai
private: false
-->

# 概要

React の状態管理の 1 つである、`Jotai`について学んだことをメモします。

# 学習内容

- [Jotai とは](#jotaiとは)
- [導入方法](#導入方法)
- [設定方法](#設定方法)

# Jotai とは

Atom ベースの状態管理。recoil の影響が強い(概念はほぼ同じ)。
ボトムアップアプローチ（コンポーネントから状態を生成する）
![jotai](https://storage.googleapis.com/candycode/jotai/jotai-mascot.png)
[^1](https://jotai.org/)

## 特長

- 簡素な記述で使える
- 最小限のコア API (2kb)
- 多くのユーティリティと統合
- TypeScript 指向
- Next.js、Gatsby、Remix、React Native で動作
- SWC および Babel プラグインを使用した React Fast Refresh

# 導入方法

## インストール方法

```
$ npm i jotai
```

or

```
$ yarn add jotai
```

# 設定方法

1. `atom()`でデータを状態管理する

```javascript
export const todosAtom = atom();
```

2. `useAtom()`により、ページやコンポーネント内で状態管理しているデータを利用

```javascript
const [_, setTodos] = useAtom(todosAtom);
```

## 使用例

```typescript :src/state/todo
import { atom } from "jotai";
import { Todo } from "src/types";

export const todosAtom = atom<Todo[]>([
  { id: 1, text: "foo", isDone: false },
  { id: 2, text: "bar", isDone: true },
]);
```

```typescript
import type { NextPage } from "next";
import { todosAtom } from "src/state/todo";
import { Todo } from "src/types";
import { useAtom } from "jotai";

const Home: NextPage = () => {
  const [todos, setTodos] = useAtom(todosAtom);

  const toggleIsDone = (id: Todo["id"]) => {
    setTodos((prevTodos) => {
      return prevTodos.map((todo) => {
        if (todo.id === id) {
          return { ...todo, isDone: !todo.isDone };
        }
        return todo;
      });
    });
  };

  return (
    <div>
      <h2>TODO一覧</h2>
      {todos.map((todo) => (
        <div key={todo.id}>
          <label style={{ fontSize: "2rem" }}>
            <input
              type="checkbox"
              checked={todo.isDone}
              onChange={() => toggleIsDone(todo.id)}
              style={{ width: "1.5rem", height: "1.5rem" }}
            />
            {todo.text}
          </label>
        </div>
      ))}
    </div>
  );
};

export default Home;
```

# Selector の利用

Recoil と同様に、Jotai の Selector である、`selectAtom`を使うことで、簡易でかつよりパフォーマンスの高い記述ができる。

1. `selectAtom`でデータの必要な要素のみを状態管理する

```javascript
export const todosLenthAtom = selectAtom(todosAtom, (todos) => todos.length);
```

2. `useAtom`により、ページ内やコンポーネント内で必要な要素のみを状態管理から取得する

```javascript
const [todosLength] = useAtom(todosLenthAtom);
```

## 使用例

データ`todos`のサイズ[`todos.length`]だけが必要な場合

```javascript
import { atom } from "jotai";
import { selectAtom } from "jotai/utils";
import { Todo } from "src/types";

export const todosAtom = atom<Todo[]>([
    { id: 1, text: "foo", isDone: false },
    { id: 2, text: "bar", isDone: true },
])

export const todosLenthAtom = selectAtom(todosAtom, todos => todos.length);
```

```javascript
import { FC } from "react";
import { todosLenthAtom } from "src/state/todo";
import { useAtom } from "jotai";

export const TodoCounter: FC = () => {
  const [todosLength] = useAtom(todosLenthAtom);
  return <h2>TODO: {todosLength}件</h2>;
};
```

# ロジックの Jotai 管理

ロジックを状態管理することで、ページやコンポーネントの記述をよりシンプルにできる。

##　使用例

```typescript
import { atom } from "jotai";
import { selectAtom } from "jotai/utils";
import { Todo } from "src/types";

export const todosAtom = atom<Todo[]>([
  { id: 1, text: "foo", isDone: false },
  { id: 2, text: "bar", isDone: true },
]);

export const toggleTodoAtom = atom<Todo[], Pick<Todo, "id">>(
  (get) => get(todosAtom),
  (get, set, update) => {
    const prevTodos = get(todosAtom);
    const newTodos = prevTodos.map((todo) => {
      if (todo.id === update.id) {
        return { ...todo, isDone: !todo.isDone };
      }
      return todo;
    });
    set(todosAtom, newTodos);
  }
);
```

```javascript
import type { NextPage } from "next";
import { toggleTodoAtom } from "src/state/todo";
import { useAtom } from "jotai";

const Home: NextPage = () => {
  const [todos, toggleTodos] = useAtom(toggleTodoAtom);

  return (
    <div>
      <h2>TODO一覧</h2>
      {todos.map((todo) => (
        <div key={todo.id}>
          <label style={{ fontSize: "2rem" }}>
            <input
              type="checkbox"
              checked={todo.isDone}
              onChange={() => toggleTodos({ id: todo.id })}
              style={{ width: "1.5rem", height: "1.5rem" }}
            />
            {todo.text}
          </label>
        </div>
      ))}
    </div>
  );
};

export default Home;
```

# 参考

[IT Kingdom](https://it-kingdom.com)<br>
[Jotai 公式ページ](https://jotai.org/)<br>
[GitHub リポジトリ](https://github.com/yamashin01/react_state_management/tree/main/Jotai01)<br>

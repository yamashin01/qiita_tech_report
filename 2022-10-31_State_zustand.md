<!--
title:   【React状態管理4】Zustand
tags:    react, statemanagement, zustand
private: false
-->

# 概要
Reactの状態管理の1つである、`zustand`について学んだことをメモします。

# 学習内容
- [Zustandとは](#zustandとは)
- [導入方法](#導入方法)
- [ミドルウェアの活用](#ミドルウェアの活用)
- [複数データの管理方法](#複数データの管理方法)

# Zustandとは
「ツースタンド」と読む。比較的容易に使えるため、人気は高い。

![zustand](https://github.com/pmndrs/zustand/blob/HEAD/bear.jpg?raw=truehttps://github.com/pmndrs/zustand/blob/HEAD/bear.jpg?raw=true)

## Reduxとの違い
- シンプルで自由度が高い
- hooksを状態管理手法に用いる
- `Providers`によるコードのラッピング不要
- レンダリングなしでコンポーネントの状態を更新する


## useContextとの違い
- コード量が少ない
- レンダリング回数が少ない
- アクションベースで状態管理を行う

# 使用方法
## 手順
1. zustandをインストールする
    ```
    $ npm install zustand
    ```
    or
    ```
    $ yarn add zustand
    ```

1. storeを作成する
    ```javascript
    import create from 'zustand'

    const useBearStore = create((set) => ({
        bears: 0,
        increasePopulation: () => set((state) => ({ bears: state.bears + 1 })),
        removeAllBears: () => set({ bears: 0 }),
    }))
    ```

1. hooksを用いてコンポーネントでstoreを呼び出す
    ```javascript
    function BearCounter() {
    const bears = useBearStore((state) => state.bears)
    return <h1>{bears} around here ...</h1>
    }

    function Controls() {
    const increasePopulation = useBearStore((state) => state.increasePopulation)
    return <button onClick={increasePopulation}>one up</button>
    }
    ```

## 使用例
`state/todos.ts`でstoreを作成し、`pages/index.tsx`や`pages/add.tsx`で状態を取得、更新するTodo管理アプリの場合

```javascript :state/todos.ts
import { Todo } from 'src/types';
import create from 'zustand'

// 管理する状態の初期値
const TODOS: Todo[] = [
    { id: 1, text: "foo", isDone: false },
    { id: 2, text: "bar", isDone: true },
    ];

// 管理する状態の型宣言
type State = {
    todos: Todo[];
    addTodo: (text: Todo["text"]) => void;
    toggleTodo: (id: Todo["id"]) => void;
}

// storeを作成する
export const useStore = create<State>((set) => ({
    // store内のtodos
    todos: TODOS,
    // store内のtodosを追加する
    addTodo: (text) => {
        set((state) => {
            const newTodo = {id: state.todos.length + 1, text, isDone: false};
            return {todos: [...state.todos, newTodo]};
        });
    },
    // store内のtodosの状態を更新する
    toggleTodo: (id) => {
        set((state) => {
            return {
                todos: state.todos.map((todo) => {
                    if (todo.id === id) {
                        return {...todo, isDone: !todo.isDone };
                    }
                    return todo;
                }),
            };
        });
    },
}));
```

```javascript :pages/index.tsx
import type { NextPage } from "next";
import { useEffect } from "react";
import { useStore } from "src/state";

const Home: NextPage = () => {
  // Hooksを用いてstore内の状態を更新する
  const todos = useStore((state) => state.todos); // store内のtodosを取得
  const toggleIsDone = useStore((state) => state.toggleTodo); // store内のtodosを取得

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

```javascript :pages/add.tsx
import type { NextPage } from "next";
import { ComponentProps } from "react";
import { useStore } from "src/state";

const Add: NextPage = () => {
  // Hooksを用いてstore内のtodosを新規追加する
  const addTodo = useStore((state) => state.addTodo);
  const handleSubmit: ComponentProps<"form">["onSubmit"] = (e) => {
    e.preventDefault();
    const text = e.currentTarget.text.value;
    addTodo(text);
    e.currentTarget.reset();
  }

  return (
    <div>
      <h3>TODO追加</h3>
      <form onSubmit={handleSubmit}>
        <input type="text" name="text" autoComplete="off" required/>
        <button>追加</button>
      </form>
    </div>
  );
};

export default Add;
```

# ミドルウェアの活用
zustandには、より効率的に状態を管理するための様々なミドルウェアが用意されている。下記では、その一部を紹介する。
## log
状態更新前後のデータ比較をログ出力できる
### 使用例
```javascript
const log = (config) => (set, get, api) =>
  config(
    (...args) => {
      console.log('  old state', get())
      set(...args)
      console.log('  new state', get())
    },
    get,
    api
  )
```

## Immer
ミュータブル（破壊的な）メソッドを簡潔に扱うことができる。<br>環境によってはインストールが必要。
### 導入方法
```
$ npm install immer
```
or
```
$ yarn add immer
```

### 使用例
```javascript
import { Todo } from 'src/types';
import create from 'zustand';
import { immer } from 'zustand/middleware/immer';

export const useStore = create<State>()(
    immer((set) => ({
        todos: TODOS,
        addTodo: (text) => {
            // immerを使うことにより、ミュータブルなメソッドであるpushを使用できる
            set((state) => {
                state.todos.push({ id: state.todos.length + 1, text, isDone: false })
            },
            false,
            "todos/addTodo"
            )
        },
    }))
);
```

## Redux DevTools
zustandでもRedux DevToolsを使うことができる（Redux Devtoolsの詳細は、Reduxの紹介ページを参照）。
### 使用例
```javascript
import create from 'zustand';
import { immer } from 'zustand/middleware/immer';
import { devtools } from 'zustand/middleware';

export const useStore = create<State>()(
    devtools((set) => ({
        todos: TODOS,
        addTodo: (text) => {
            set((state) => {
                const newTodo = {id: state.todos.length + 1, text, isDone: false};
                return {todos: [...state.todos, newTodo]};
            },
            )
        },
    }))
);
```

# 複数データの管理方法
複数のデータを状態管理したい場合、下記が推奨されている。
- storeは一つにすること
- storeの更新には`set`を用いること
- 各データはsliceで個別に保存し、一つのstoreでまとめて状態管理すること

## 使用例：todosとusersの二つのデータを状態管理したい場合
各データの状態は、それぞれslice関数`createTodosSlice()`, `createUsersSlice()`で管理し、`useStore()`という関数で一つのstoreにまとめる
- 各データで共通して利用するミドルウェアも`useStore()`で設定すれば良い
- 各Slice関数の引数では`StateCreator()`という機能を利用する
  - 第一引数：状態管理するすべてのデータの型
  - 第二引数：利用するすべてのミドルウェアの配列
  - 第三引数：配列
  - 第四引数：本関数で管理するデータの型

1. 各slice関数をまとめるstoreを生成する
    ```javascript :state/index.ts
    import { immer } from "zustand/middleware/immer";
    import { devtools } from "zustand/middleware";
    import create from "zustand";
    import { createTodosSlice } from "./todos";
    import { State } from "./types";
    import { createUsersSlice } from "./users";

    // 各slice関数をstoreにまとめる
    // 共通して利用するミドルウェアもここで実装する
    export const useStore = create<State>()(
        devtools(
            immer((...args) => {
                return {
                    ...createTodosSlice(...args),
                    ...createUsersSlice(...args),
                }
            })
        )
    );
    ```

1. 各slice関数を宣言する
    ```javascript :state/todos.ts
    import { StateCreator } from 'zustand';
    import { State, UsersState } from './types';

    import { Todo } from 'src/types';

    const TODOS: Todo[] = [
        { id: 1, text: "foo", isDone: false },
        { id: 2, text: "bar", isDone: true },
        ];

    // slice関数を定義する
    export const createTodosSlice: StateCreator<
        State,  // すべてのデータの型(TodosState & UsersState)
        [["zustand/devtools", never],["zustand/immer", never]],   // 利用するすべてのミドルウェアの配列
        [], // 配列
        TodosState // createTodosSlice関数で管理するデータの型
    > = (set) => ({
        todos: TODOS,
        addTodo: (text) => {
            set((state) => {
                state.todos.push({ id: state.todos.length + 1, text, isDone: false })
            },
            false,
            "todos/addTodo"
            )
        },
        toggleTodo: (id) => {
            set((state) => {
                state.todos.forEach((todo: Todo) => {
                    if (todo.id === id) {
                        todo.isDone = !todo.isDone;
                    }
                })
            }, 
            false, 
            "todos/toggleTodo"
            );
        },
    });

    // `async/await`の非同期処理によりデータを取得する
    export const createUsersSlice: StateCreator<
        State, 
        [["zustand/devtools", never],["zustand/immer", never]], 
        [], 
        UsersState
    > = ((set) => ({
        users: [],
        fetchUsers: async () => {
            const response = await fetch("https://jsonplaceholder.typicode.com/users");
            set({ users: await response.json() }, false, "fetch/fetchUsers");
        },
    }));
    ```

# 参考
[IT Kingdom](https://it-kingdom.com)<br>
[GitHubリポジトリ](https://github.com/yamashin01/react_state_management/tree/main/Zustand03)<br>
[zustandパッケージ](https://www.npmjs.com/package/zustand)<br>
[zustand GitHubコード](https://github.com/pmndrs/zustand)
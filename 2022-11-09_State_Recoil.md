<!--
title:   【React状態管理6】Recoil
tags:    react, statemanagement, recoil
private: false
-->

# 概要
Reactの状態管理の1つである、`Recoil`について学んだことをメモします。

# 学習内容
- [Recoilとは](#recoilとは)
- [導入方法](#導入方法)
- [selectorを用いる利点](#selectorを用いる利点)

# Recoilとは
Meta製の、Atomsベースの状態管理ライブラリ。<br>
Selectorsを使うことで、複数のatomを合わせた状態を利用することもできる。

![Recoilの構成図](https://res.cloudinary.com/practicaldev/image/fetch/s--8ilIU9z_--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jv4d83g1xf9nhn3jewco.png)
[^1](https://dev.to/coleredfearn/atomos-a-new-recoil-visualization-tool-powered-by-react-flow-4b6l)

## 特長
- 比較的容易に利用できる
- Atomベースのため、各データの状態を個別に管理できる


# 導入方法
## インストール方法
```
$ npm install recoil
```
or
```
$ yarn add recoil
```

## 設定方法
1. 最上位の関数を`RecoilRoot`でラップする
    ```javascript
    import React from 'react';
    import {
        RecoilRoot,
        atom,
        selector,
        useRecoilState,
        useRecoilValue,
    } from 'recoil';

    function App() {
        return (
            <RecoilRoot>
                <CharacterCounter />
            </RecoilRoot>
        );
    }
    ```
1. `atom()`メソッドで状態のオブジェクトを追加する<br>
    `key`：atomのキー(アプリ内でユニークである必要がある)<br>
    `default`：状態の初期値
    ```javascript
    const textState = atom({
        key: 'textState', // unique ID (with respect to other atoms/selectors)
        default: '', // default value (aka initial value)
    });    
    ```
1. `useRecoilState`で状態を取得、更新する<br>
    `useRecoilState`の使い方は、基本的に`useState`と同じ。下記では、文字入力により`textState`の状態を更新している。
    ```javascript
    function CharacterCounter() {
        return (
            <div>
                <TextInput />
                <CharacterCount />
            </div>
        );
    }

    function TextInput() {
        const [text, setText] = useRecoilState(textState);

        const onChange = (event) => {
            setText(event.target.value);
        };

        return (
            <div>
                <input type="text" value={text} onChange={onChange} />
                <br />
                Echo: {text}
            </div>
        );
    }
    ```
1. `selector`であるatomの状態を変換し、`useRecoilValue`で取得する<br>
    下記では、文字データ`textState`を`selector`で文字データの長さ`charCountState`に変換し、`useRecoilValue`で取得している。
    ```javascript
    const charCountState = selector({
        key: 'charCountState', // unique ID (with respect to other atoms/selectors)
        get: ({get}) => {
            const text = get(textState);

            return text.length;
        },
    });

    function CharacterCount() {
        const count = useRecoilValue(charCountState);

        return <>Character Count: {count}</>;
    }
    ```

# `selector`を用いる利点
- シンプルに記述できる
- パフォーマンスが向上する<br>
レンダリングを行う場所を更新箇所のみにできる

# 参考
[IT Kingdom](https://it-kingdom.com)<br>
[Recoil公式ページ](https://recoiljs.org)<br>
[ATOMOS](https://dev.to/coleredfearn/atomos-a-new-recoil-visualization-tool-powered-by-react-flow-4b6l)<br>
[GitHubリポジトリ](https://github.com/yamashin01/react_state_management/tree/main/Recoil01)<br>
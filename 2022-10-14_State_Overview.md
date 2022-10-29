<!--
title:   【React状態管理1】状態管理とは
tags:    react, statemanagement
private: false
-->

# 概要
Reactの状態管理について学んだことを共有します。
本稿では状態管理の全体像の話ですが、今後、各状態管理ライブラリの使い方について投稿する予定です。

# 学習内容
- [状態管理とは](#状態管理とは)
- [状態管理のライブラリの種類](#状態管理のライブラリの種類)

# 状態管理とは
プロジェクト内のデータを専用の領域で保持・管理したもの（React Hooksでよく使われる`useState`も単一コンポーネントでの状態管理と言える）。
状態管理を使わなくても設計は可能だが、複数のコンポーネントでデータを活用する場合、下記のようにバケツリレー的にデータの受け渡しが必要となり、コードが煩雑になる。
そのため、規模の大きなプロジェクトほど、状態管理が重要となる。

![Reactのバケツリレー](https://camo.qiitausercontent.com/6e6427219ac7bbe8ce6af9c5681d2617e65c0ede/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e61702d6e6f727468656173742d312e616d617a6f6e6177732e636f6d2f302f3430363732372f35323537373032392d316233362d636366662d616562352d3363646462333736346430362e706e67)[^1]
[^1]: [Rectの状態管理のベストは！？](https://qiita.com/yoshinoritera55/items/57a14fc08641631452bc)より引用
<center>データ受け渡しのバケツリレー</center>

# 状態管理のライブラリの種類
状態管理のライブラリは複数あるが、それぞれ一長一短があり、主流と言えるライブラリはない。そのため、状況に応じて利用するライブラリを選定する必要がある。
| ライブラリ名 | 内容 | ReactAPI？ | ベース |
| --- |---|---|---|
|useContext|Reactの公式フック|Yes||
|Redux|これまで最も主流。非常に有用だが、概念理解が難しい。|Yes|state machine based|
|Zustand|シンプルで使いやすい|No|state machine based|
|XState|ステートマシンを実装するのに特化したライブラリ。状態遷移の制御が容易||state machine based|
|Recoil|Meta社が開発したライブラリ。|Yes|atom based|
|Jotai|サイズが軽量で利用が比較的容易|Yes|atom based|
|valtio|Poimandresが開発しているライブラリ|No|atom based|
atom-based：状態を個別に管理する
state machine based：全体を一つの状態として管理する


# 参考
[IT Kingdom](https://it-kingdom.com)
[状態管理とは](https://zenn.dev/maromero/scraps/ceafacc6c27662)

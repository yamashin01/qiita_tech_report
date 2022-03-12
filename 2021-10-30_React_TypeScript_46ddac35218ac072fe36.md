<!--
title:   typescript環境開発時のエラー（初心者向け）
tags:    React,TypeScript,エラー
id:      46ddac35218ac072fe36
private: false
-->

# 概要

備忘録がてら、Typescript を使った React 開発環境を構築する時に発生したエラーと対策をメモします。

# 手順

1. プロジェクトを作るフォルダに移動する
1. 下記コマンドを実行する

```
npx create-react-app . --tempalte typescript
```

# エラー発生

上記の設定で`npm start``を実行すると、下記のエラーが発生

```
Could not find a declaration file for module ‘react’.
```

原因を確認すると、どうも React が Typescript で書かれていないため、型提供が必要とのこと。<br>
下記コマンド実行することで解消しました。

```
yarn add -D @types/react
```

# 参考資料

[こちら](https://qiita.com/waniwaninowani/items/7ea8b4eacab296758c19)の記事を参考にしました。

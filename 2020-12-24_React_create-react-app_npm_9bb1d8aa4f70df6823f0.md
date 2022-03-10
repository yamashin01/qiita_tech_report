<!--
title:   Reactの開発環境を作る
tags:    React,create-react-app,npm
id:      9bb1d8aa4f70df6823f0
private: false
-->
#Reactの開発環境

ターミナル上で下記のコマンドを実行することで、create-react-appを準備する

##create-react-appとは
Reactを開発するための環境


##準備する環境
macbook
node 8.10以上
npm 5.6以上

---
##開発環境を作るディレクトリに移動する

##homebrewをインストールする

##nodebrewをインストールする
一緒にnpmもインストールされる
###インストールコマンド
brew install nodebrew

###インストールの確認
nodebrew -v

##npmインストールの確認
npm -v

##reate-react-appのインストール
npx create-react-app [開発環境を作るディレクトリ名]

上記作業により、開発環境は完了

---
#create-react-appの実行
## npmのビルド
npm run build

## npmの開始
npm start
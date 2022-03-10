<!--
title:   Vercelでデプロイする方法（基本的な流れ）
tags:    GitHub,Vercel,deploy
id:      b5e9dd03ad88b0e289a1
private: false
-->
## 概要
Next.jsについて学ぶ中で、開発したWebアプリを簡単に公開する方法として、Vercelを使った方法を知ったので、備忘録として記載しておきます。

## Vercelとは
Vercel社が開発した、Webサイトやアプリを公開するためのプラットフォーム。
ちなみに、Vercel社はモダンなreact開発のフレームワークとして人気のNext.jsも提供しており、Vercelとの相性も非常に良い。

## Vercelの特徴
- ユーザーへの公開が容易
- Githubと連動して公開可能
- 基本、無料で利用できる

## Vercelでのデプロイの手順
0. Vercelサイトでサインアップ<br>
下記のVercelサイトの右上の"Sign Up"から、Gitアカウントでサインアップする<br>
https://vercel.com/
0. サインアップ画面での右上にある"New Repository"をクリックする
0. 画面左の"Import Git Repository"から、公開したいプログラムを選択する
0. "Select Vercel Scope"で、公開元となるアカウントを選択する<br>
# 基本は、自分の個人アカウントで良い
0. "Project Name"を指定し、右下の"Deploy"をクリックする<br>
# 他の項目はひとまず操作不要

上記作業後、自動でデプロイ処理が実行されます。紙吹雪が舞って、"Congraturations"と表示されたらアプリの公開は完了です。

超簡単！！！！
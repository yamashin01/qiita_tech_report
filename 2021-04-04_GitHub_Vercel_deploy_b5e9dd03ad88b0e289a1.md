<!--
title:   Vercelでデプロイする方法（基本的な流れ）
tags:    GitHub,Vercel,deploy
id:      b5e9dd03ad88b0e289a1
private: false
-->

# 概要

Next.js について学ぶ中で、開発した Web アプリを簡単に公開する方法として、Vercel を使った方法を知ったので、備忘録として記載しておきます。

# Vercel とは

Vercel 社が開発した、Web サイトやアプリを公開するためのプラットフォーム。
ちなみに、Vercel 社はモダンな react 開発のフレームワークとして人気の Next.js も提供しており、Vercel との相性も非常に良い。

# Vercel の特徴

- ユーザーへの公開が容易
- Github と連動して公開可能
- 基本、無料で利用できる

# Vercel でのデプロイの手順

0. Vercel サイトでサインアップ<br>
   下記の Vercel サイトの右上の"Sign Up"から、Git アカウントでサインアップする<br>
   https://vercel.com/
1. サインアップ画面での右上にある"New Repository"をクリックする
2. 画面左の"Import Git Repository"から、公開したいプログラムを選択する
3. "Select Vercel Scope"で、公開元となるアカウントを選択する<br>
   基本は、自分の個人アカウントで良い
4. "Project Name"を指定し、右下の"Deploy"をクリックする<br>
   他の項目はひとまず操作不要

上記作業後、自動でデプロイ処理が実行されます。紙吹雪が舞って、"Congraturations"と表示されたらアプリの公開は完了です。

超簡単！！！！

<!--
title:   Firebaseにデプロイするまで
tags:    Firebase,React,deploy
id:      5fa79cbebe2fe54e2b97
private: false
-->
#Firebaseの設定
## Firebaseでプロジェクトを立ち上げる
1. ブラウザでFirebaseのコンソール(https://console.firebase.google.com/u/0/?hl=ja) へアクセスし、Googleアカウントでログインする
2. 指定の**プロジェクトを追加**をクリックする
3. アルファベットの適当なプロジェクト名を設定する
4. Googleアナリティクスを有効にして続行する
5. アナリティクスのアカウントを作成してプロジェクトを作成する

## プロジェクトの初期設定を行う
1. プロジェクト画面で、web設定(**</>**ボタン）をクリックする
2. アプリのニックネーム(上のプロジェクト名と同様で良い）を設定して**次へ**をクリックする
3. **Firebase SDKの追加**で、**次へ**をクリックする
4. **Firebase CLIのインストール**で、表示されたコマンド(npm install -g firebase-tools)をPCのコマンドラインで実行する
※過去に実行したことがある場合は不要
5. **Firebase Hosting Deploy**で**次へ**をクリックする

## プロジェクトの設定を行う
1. プロジェクト画面の右上の設定ボタン（左上の歯車ボタン）をクリックする
2. リソースロケーションで、**asia-notheast1**を選択し、**完了**をクリックする
※ここは一度設定すると変更不可のため注意
3. 左の**Cloud Firestore**をクリックする
4. **データベースの作成**をクリックする
5. **本番環境の開始**を選択して続行する
6. **ロケーションの選択**は変更せず**完了**をクリックする

# ターミナルでの設定
## ローカルのディレクトリとFirebaseを紐付ける
1. ローカルPCにアプリを作るディレクトリを作る
2. コマンドラインで作成したディレクトリに移動する
3. コマンドラインで、**firebase login**と入力してEnterする
4. ブラウザ上でGoogleアカウントを選択する画面に移動したら、上でしていたGoogleアカウントを選択して許可をする
5. コマンドラインで、**firebase init**と入力してEnterする
6. firebaseで使用するサービスが表示されるため、矢印キーとスペースキーで下記を選択してEnterする
 - Firestore
 - Functions
 - Hosting
 - Storage
7. 使用するプロジェクトとして、新規プロジェクトor既存のプロジェクトを聞かれるため、既存のプロジェクト **Use an existing project**を選択してEnterする
8. ブラウザのFirebase画面で生成したプロジェクト一覧が表示されるため、上で生成したプロジェクト名を選択してEnterする
9. Firestoreで使用するルール、Firestoreのindexを聞かれるので、デフォルトの設定のまま（何も入力せず）、Enterする
10. 使用する言語として**JavaScript**と**TypeScript**の選択が表示されるため、得意な言語を選択する
11. ESLintを用いるか、依存関係のあるパッケージのインストールを聞かれるため、どちらもデフォルトの設定（ESLintは使用しない、パッケージはインストールする）のままEnterする
12. publicディレクトリを聞かれるため、**build**と入力してEnterする（デフォルトではpublic)
13. SinglePageAppとして設定するかを聞かれるので、**yes**と入力してEnterする（デフォルトではNo）
これでFirebaseとの紐付けは完了

## 必要なパッケージの追加
開発に必要と思われるパッケージをnpmインストールする。
コマンドラインで、下記のコマンドを実行する(XXXXX, YYYYY, ZZZZZに必要なパッケージ名を記入)
`npm install --save XXXXX YYYYY ZZZZZ`

開発におすすめのパッケージ

| パッケージ名 | 内容 |
|:-:|:-:|
|@material-ui/core   |material-uiの基本   |
|@material-ui/icons   |material-uiのアイコン   |
|@material-ui/styles   |material-uiのスタイル   |
|connected-react-router   |Reactルータのパッケージ   |
|firebase   |Firebase   |
|history   |URL遷移の記録   |
|react-redux   |reduxのパッケージ   |
|react-router   |ルーティングのパッケージ   |
|redux   |redux   |
|redux-actions   |reduxのactions   |
|redux-logger |reduxのstateの変化のログ |
|redux-thunk |reduxの非同期処理を待つ |
|reselect| |

上記を実行すると、ディレクトリ内のpackage.jsonに上記が追加される

# デプロイの実行
## デプロイして公開する
1. コマンドライン上で**npm run build**と入力して実行し、ビルドを行う(公開できるようにファイルを変換する)
2. **firebase deploy**を実行する

*firebaseのプランを無料から従量制に変更しておかないと**HTTP ERROR: 400**と表示される
3. コマンドライン上に表示されたURLをコピーしてブラウザで実行すると、生成した画面が生成される
<!--
title:   VSCodeからVagrantにリモートアクセスする
tags:    SSH,Vagrant,VisualStudioCode
id:      87e042b3f5ec52e57017
private: false
-->
## 概要
VisualStudioCodeからVagrantに直接リモートアクセスできることを知ったので、早速トライしてみました。
が、意外とハマってしまったので、備忘録がてらメモを残します。

## 前提
- Vagrant, VirtualBoxをインストールしていること
- Vagrantによる仮想サーバーの設定を完了していること

## 手順
1. "vagrant up"コマンドで仮想サーバーを立ち上げる(WindowならコマンドプロントかPowerShell、Macならターミナルから起動)
1. 拡張機能"RemoteDevelopment"をインストールする
<img width=400 alt="VSCode拡張" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/656610/acaf06a7-6556-92b1-596b-c6d586a2dfe7.png">
インストール後、画面左側にSSH用のアイコンが追加されます。

1. アイコン"Remote Explorer"をクリックし、画面左上のリモートエクスプローラーのプルダウンから`SSH Target`を選択する
1. 画面左上の設定ボタン(歯車アイコン)をクリックし、コンフィグファイルを選択する
通常は下記のリンクを選択します。<br>
`Users¥username¥.ssh¥config`

1.  リモートアクセスするサーバー情報を追加する
表示されたテキストデータの一番下の行で1行改行し、下記の情報を追加する

```
HostName  XXXXXXX(ここの名称は何でもよさそう)
User XXX(vagrantにアクセスするユーザ名)
Port 22(設定したポート番号)
IdentityFile C:¥username¥Documents¥XXXX(秘密鍵のファイルパス。通常、.vagrantディレクトリ配下にある様子)
```

ここの設定に失敗していたため、アクセスに何度も失敗しました。
ここの情報は、ターミナルの下記コマンドで確認する必要があります。
`vagrant ssh-config`
ここで出てきたHostName、User、Port、IdentityFileを設定すれば良いようです。
上記設定により、画面左側の"SSH TARGETS"内に、HostNameで設定したホスト名が追加されます。

1.  新規に追加されたホスト名を右クリックし、"Connect to Host in Current Window"をクリックする
最初の接続時には、下記を選択する
    - OSを聞かれるのでVagrantで設定したOSを選択する
    - Continueを選択する

上記完了後、VSCodeでvagrant上のディレクトリを開くことができるようになります。
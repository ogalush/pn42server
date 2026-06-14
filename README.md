# pn42server構築
## Overview
2026.6に購入したASUS pn42 の踏み台サーバの構築Playbook.  
2013年から利用の [ECS Liva](http://www.ecsjpn.co.jp/liva/liva2.htm) の後継.

## 何を入れるか
今まで使っていた機能を引き継ぐ.
* MyDNS
* WireGuard
* docker compose
* bind9


# 環境
|Key|Value|
|---|---|
|Model|[ASUS ExpertCenter PN42](https://www.asus.com/jp/displays-desktops/mini-pcs/pn-series/asus-expertcenter-pn42/)|
|OS|Ubuntu 26.04|
|IP address|192.168.3.221|

# 事前準備
Ubuntu 26.04をインストール済であること.

# インストール手順
## 初期セットアップ
OSの基本設定や基礎パッケージのインストールは従来のPlaybookを利用する
### git準備
```
$ sudo apt -y install git
```
### 初期パッケージのインストール
アカウント、グループ、sudo設定、dotファイルの初期設定.
```
$ mkdir -v ~/git
$ cd ~/git
$ git clone git@github.com:ogalush/Anything.git
$ cd Anything
$ ansible-playbook -i '192.168.3.221,' initialize_instance.yml -bK --list-hosts
$ ansible-playbook -i '192.168.3.221,' initialize_instance.yml -bK --check --diff
$ ansible-playbook -i '192.168.3.221,' initialize_instance.yml -bK
→ インストール成功すること.
```

## 構築Playbookの実行
```
$ ansible-playbook -i dev.ini install_pn42.yml -bK --list-hosts
$ ansible-playbook -i dev.ini install_pn42.yml -bK --ask-vault-pass --check --diff
$ ansible-playbook -i dev.ini install_pn42.yml -bK --ask-vault-pass
```

## 接続確認
### WireGuard
Clientファイルを端末へ読み込んで接続する。IPv6経由。
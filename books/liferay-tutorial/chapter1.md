---
title: "Quick Start"
free: false
---

## Dockerのインストール
Dockerのインストールは[Macはこちらから](https://docs.docker.com/docker-for-mac/install/)、[Windowsはこちらを参考に](https://docs.docker.com/docker-for-windows/install/)インストールしてください。

## Liferay + MySQLの起動
1. [このリポジトリ](https://github.com/yasuflatland-lf/liferay-quickstart)をローカルに`git clone`もしくはダウンロードしてきます。
2. 解凍したルートディレクトリで、
```
docker-compose -f docker-compose-ce.yml up --build
```
  を実行してください。MySQL、Liferay CEが起動します。
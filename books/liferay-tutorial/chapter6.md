---
title: "カスタム開発の基礎 (TBD)"
free: false
---

# Liferay開発ツールの紹介
Liferay開発では、Liferayが提供している開発ツールを利用することによって、大きく開発効率をあげることができます。詳しい情報は各キーワードで検索してもらうとして、ここでは各ツールのキーワード、それぞれの利点に絞って紹介します。
## Liferay Workspace
Liferayを開発するために最適化されたワークスペースです。Liferay Workspaceを利用しなくても開発は可能ですが、以下の利点があり、特別な理由がない限り、Liferay Workspaceを利用するのが開発のベストプラクティスとなります。

### Bladeとの統合
Liferay開発ではJavascript、Javaなど複数の言語、開発環境を利用しますが、Liferayが規定したベストプラクティスのディレクトリ構造に沿って開発を効率的に進めることができます。

### BOMの設定
Liferayでは複数のライブラリを利用しており、バージョンやマイナーバージョン(サービスパック、フィックスパック、GA1, GA2など)によって、対応するライブラリが異なります。それを手動で追従するのは非常に面倒ですし、バージョンをあげたいだけのために、`build.gradle`ファイルや`bnd.bnd`ファイルを変更するのも運用として非効率です。

そうした問題を解決するのがBOM(Bill of Materials,部品表)と呼ばれる、依存ライブラリバージョンをパッケージとしてバンドルしたライブラリです。LiferayでBOMを利用するには`Liferay Workspace`のルートディレクトリの、`gradle.properties`内部に
```
liferay.workspace.product=dxp-7.3-ga1
```
というようにターゲットとなるLiferayバージョンを記述してやることによって、自動的にそれ以下のプロジェクトでLiferay関連のライブラリの依存バージョンをBOMを利用して自動的に指定してくれます。

7.4以降、フィックスパック、サービスパックや新規マイナーバージョンリリース毎に新機能が追加されてくる予定で、そうしたライブラリ依存関係の解決が煩雑になると予想されています。開発の効率化のために、ターゲットバージョンの指定は忘れないようにしましょう。

## Blade
BladeはLiferay開発における根幹となるツールで、`gradle`をラップしたツールです。jarファイルの生成、ローカル環境へのデプロイ、調査、ライブラリ依存関係の調査など、LiferayにおけるカスタムAPI, Widget開発には必須のツールとなります。

LiferayではWindows, Mac, Linux用のインストーラーを準備しているので、それを利用してインストールできます。詳しくは[公式サイトの情報]()を参照ください。
## Liferay Developer Studio / Liferay IDE
Liferay謹製のEclipseベースのIDEです。Liferay Developer Studio(LDS)はEEユーザーのみ利用可能で、Liferay IDEは無料版ユーザーが利用可能です。違いとしてはLDSはLiferay Workflow (XMLデータ）のGUIからの編集機能が付属しているくらいで、GUIでのWorkflow編集機能もLiferay本体(Liferayにログインして利用）に付属しているので、無料版ユーザーでもGUI編集が利用可能です。

サービスビルダー、BladeといったLiferayのツール群と統合されているので、IDEの新規作成から、項目を設定していくことで、GUIからそれらツール群を利用することができたり、生成したjarファイルのデプロイなども可能です。

IntelliJをすでに利用していて、そちらが慣れている、というような理由がなければ利用してください。
## IntelliJ IDEA Plugin
すでにIntelliJ IDEA を別の開発でも使っており、そちらが使い慣れている場合は、Liferayから公式のIntelliJプラグインもリリースしています。`Preferences -> Plugins`から`Liferay`と検索すると、プラグインが選択できますので、インストールしてください。

あとは該当のプロジェクト、もしくは`Workspace`のディレクトリを指定して`IntelliJ`を開くと、`gradle`や`maven`のファイルを検知して自動的に依存性を解決してくれるはずです。

# 開発フローのベストプラクティス
さまざまな開発ツールがありますが、どういう組み合わせで使うのが一番効率がいいのだろうか？Liferay USのコンサルタントらの利用方法から得たベストプラクティスとしては以下になります。

## サーバーは別建て
IDEからLiferayサーバーを立てることもできますが、開発中では本番、ステージング、ローカル環境と複数の環境を使い分けることも多いかと思います。それも踏まえるとサーバーを別建てとした方がターゲットのLiferayを使い分けやすいです。

## デプロイはコマンドラインから
デプロイはコマンドラインから`blade deploy`コマンドで実行するのが早い。本番開発用、テスト開発用、一時的に試す用のバンドル、みたく複数のLiferayを使い分けることで開発スピードが向上する。

## Liferay Workspaceは各環境毎に持つのが効率的
`Liferay Workspace`の項で説明したように、現在は`gradle.properties`にターゲットLiferayのバージョンを指定して利用するのが一般的となっています。それらを毎回切り替えるのは多くのライブラリを再度ロード、結合する手間が増えるので、ローカル開発、本番、ステージングなど、デプロイ先の環境毎に`Liferay Workspace`を分けてしまうのがおすすめです。
# Service Builder
Ruby on RailsやLaravelなど、モダンなWebフレームワークにはデータベースへの書き込み、オブジェクトのマッピングを管理するスカッフォールディング機能がありますが、`Service Builder`はLiferayにおけるスカッフォールディングを担う機能になります。

Liferayでは画面上に配置するウィジェット以外にも、APIを提供したり、`Fragment`や`Form`のカスタムフィールド定義などがありますが、`Service Builder`を利用するとこうした多岐にわたるコードのスケルトンを生成することができます。

この章ではその中でもAPI開発には欠かせない、`service-builder`サービスの説明をとりあげます。
## Service Builderサービスの使い方
### service.xmlの説明
### コマンドラインでのビルド方法
### ビルドターゲットの種類
```
blade create service
```
で表示されますが、よく使うものに絞って紹介します。
| 名称 | 詳細 |
|:-- |:--|
| api | JAX-RS API作成用。特に以下に当てはまらないケースの場合にも選択する。 |
| control-menu-entry | コントロールメニューに追加するウィジェットを開発する際に利用。 |
| form-field | カスタムフォームフィールドを開発する際に利用 |
| fragment | カスタムフラグメントを作成する際に利用。GUIからもフラグメントは作成できるが、この方法だとgitなどでコードを管理できる。 |
| mvc-portlet | JSPベースのウィジェットベース。通常のウィジェットはこれをベースにする。 |
| npm-angular-portlet | Angulerベースのウィジェット作成 |
| npm-react-portlet | Reactベースのウィジェット作成 |
| npm-vuejs-portlet | Vueベースのウィジェット作成 |
| panel-app | 管理画面側に表示されるウィジェット |
| rest-builder | REST API (GraphQL、OpenAPI)インターフェースを作成する。通常`Service Builder`を併用して、そこで作成したサービスを裏で呼ぶような構成にする。 |
| service | JAX-RS API作成用。 |
| service-builder | カスタム開発でもっとも利用する。データベースへのアクセス(永続層)、それを利用するサービス |
| service-wrapper | 既存サービスの拡張時に利用する、Wrapperの実装 |

## LocalService, Serviceの役割
Liferayでは、サービス層のアクセスに`LocalService`, `Service`とサフィックスがつくクラスを利用しますが、それぞれ以下の役割があります。基本的に、ライブラリとして利用する場合は`Service`を使います。

例えば、`Service Builder`で`TodoLocalService`, `TodoService`というサービスを生成した場合、`TodoService`内部で`TodoLocalService`をコールし、権限チェックを行う、といった感じです。

|名称|役割|
|:--|:--|
| LocalService | サービスの本体。ドメインモデルにおけるユースケース、サービスを実装する。Serviceからのみコールされる。|
| Service | LocalServiceに対して権限チェックを行っているラッパー。外部から利用する際はこちらを利用する。|
| ServiceWrapper | Liferay本体が提供するクラス、例えば`UserService`などを拡張したい場合、この`Wrapper`拡張ポイントを利用できます。|
| ServiceUtil | JSPや`Web Content`の`Service Locator`からなど、静的クラスとしてのみコールできる場合に利用します。ただし、これを使ってしまうとOSGiの恩恵が受けられない（サーバーを動的に動かしたままライブラリを切り離せなくなる）ので、基本は`Service`を利用してください。|


## Workflowの組み込み
## Indexerの組み込み
## Asset Frameworkの組み込み
### コメント
### スター
### ゴミ箱
# REST Builder
## REST Builderの作り方と使い方
## GraphQL、その他呼び出し方の違い
## JAX-RSとREST Builderの大きな違い
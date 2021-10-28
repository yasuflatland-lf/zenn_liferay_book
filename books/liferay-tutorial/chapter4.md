---
title: "Liferayの歩き方 (TBD)"
type: "tech"
topics: ["liferay", "java", "portal", "osgi"]
published: true
---

# はじめに
Liferayはオープンソースで、コードが公開されていますが、コードベースが巨大なため、コードを探索するのに、その構成を理解しておくことで素早くコードを見て回れるようになります。この章ではLiferayの歩き方と称して、Liferayのコードベースの見方を紹介します。
# Liferayの種類
Liferayにはオープンソースバージョン(CE)と有償版(DXP)の２つが存在します。
| 種類| リポジトリ |
|:-- |:--|
| Liferay CE | [https://github.com/liferay/liferay-portal](https://github.com/liferay/liferay-portal) |
| Liferay DXP | [https://github.com/liferay/liferay-portal-ee](https://github.com/liferay/liferay-portal-ee) |


有償版(DXP)にはよく使われる機能やサービスとして、
| 種類| 説明 |
|:-- |:--|
| SAML 2.0 Plugin | SAML2.0形式でADFSやOktaなどと連携が可能。Liferayは通常SPとして利用する。SPとして複数IdPとの接続(IdP Initiative / SP Initiativeの両方)もできる。Liferay自体もIdP/SP両方で設定可能だが、バージョンアップやセキュリティアップデートなどの実運用を考えて、**LiferayをIdPとして利用するのは推奨しない。(開発時のテスト用などならOK)** |
| 多要素認証 | Google Authenticator を利用した多要素に対応 |
| Liferay Cluster | 本番環境にはほぼ必須のLiferayのEhcacheクラスタリング機能 (CEでもクラスター機能はあるが、EEだとパフォーマンスがいい仕組みのクラスタリングが提供される。) |
| フィックスパック | 1〜2ヶ月に一回、複数のバグ修正がバンドルされたパッチがリリースされる |
| サポート | 問い合わせ数無制限のサポートチケットが利用できる |

が付属してきます。詳しくはは[ホワイトペーパー](https://www.liferay.com/en-AU/resources/product-info/Comparativo+de+Produtos+-+Liferay+DXP+7.3+vs+Liferay+Portal+CE+7.3)を参考ください。

# OSGi
LiferayはEclipseでも採用されている、OSGiフレームワークを利用しています。OSGiは、動的なコンポーネント・モデルを実装するJavaプログラミング言語用のモジュラー・システムとサービス・プラットフォームです。

アプリケーションやコンポーネントは、展開用のバンドル(jar)の形で提供され、再起動を必要とせずにリモートでインストール、起動、停止、更新、およびアンインストールできます。起動、停止といったアプリケーションライフサイクルがインターフェースとして提供されており、その実装を作成し、Jarファイル内部に`MANIFEST.MF`を含むことで、OSGiバンドルと呼ばれる、OSGiフレームワーク上にデプロイできるJarファイルが作成できます。

`MANIFEST.MF`は複雑なフォーマットになりますが、Liferayが提供する`blade`ツールを利用してビルドすることで、そうした依存関係や、必要な定義は`bnd.bnd`ファイルという定義ファイルをベースに自動展開されます。
# コード規約
Liferayのコード、ファイル名などに規約があります。その辺を理解すると、コードの理解が素早くできるようになります。以下に主な規約に関して説明していきます。

## プロジェクト構成
## コマンドファイル

## ドメインモデル
Liferayは以下のような3層アーキテクチャーで構成され、各層からは隣り合う層にのみアクセスできるような構成になっています。
| ドメイン| 説明 |
|:-- |:--|
| プレゼンテーション層 | Reactなどのフロントエンド、Widgetなどのプレゼンテーションレイヤー。各プロジェクトの *`*-web`* という名前のディレクトリ以下に格納されている。 |
| サービス層| *`Service`* といった名前の付くクラス群。ドメインモデルでのUseCase、サービスに相当する |
| 永続化層 | *`Repository`* といった名前の付くクラス群。データーベースのテーブルを表すオブジェクト。Liferayの場合は`Hibernate`でテーブルからオブジェクトにマッピングしている。|

これらサービスや永続化層は、非常に多くのコードで構成されていますが、Liferayではボイラープレートコード（何度も繰り返し書くコード）は自動生成することができます。そうした生成を支援するのが次に説明するサービスビルダーです。
## サービスビルダー
Liferay上でAPIをカスタム開発する場合に必ず利用するのが、サービスビルダー(`Service Builder`)です。`service.xml`というXML設定ファイルをベースに、永続化層、サービス層を自動生成します。

`Liferay IDE` (CE版)、`Liferay Developer Studio(LDS)`(EE版)といったEclipseベースのLiferay用IDEを利用すると、GUI上から項目を指定しながら作成することもできますが、ここではコマンドラインから簡単なサービスを生成する方法を紹介します。

詳しくは公式ドキュメントのツール

## RESTビルダー
RESTビルダーはREST, GraphQLといったモダンAPIに必要なインターフェースをサービスビルダー同様、定義ファイルから自動生成します。RESTビルダーはAPIのフロントエンドのみを生成するので、そこにサービスビルダーで生成したAPIを内部的にコールすることで利用できるようになります。

# コードの追い方
Liferayのコードは数十万行にも及び、あたりを
## 該当リポジトリを開く
1. `SAML Plugin`など、EE版のみのコードの場合は、[liferay-porta-ee](https://github.com/liferay/liferay-portal-ee)リポジトリにアクセス、それ以外の共通コードの場合は[liferay-portal](https://github.com/liferay/liferay-portal)にアクセスする。
1. 該当バージョンが`7.4`の場合は、`7.4.x`のブランチに行く(EE, CEとも共通)
1. `github.com`上で、.(コンマ)を入力すると、`github.dev`モードとなり、Visual Studio Codeがブラウザ上で立ち上がる。

## コードの在処のあたりをつける
慣れてくると以下のどの手法で一番早くコードに辿り着けるかわかるようになるが、大きく分けて以下の方法がある。
### 直接コードのあたりをつける
例えば`SAML Widget`のコードを探したいとする。
1. SAML WidgetはEE版のみのコードなので、`liferay-portal-ee`に格納されていると仮定。
1. バージョンは`7.4`なので、`7.4.x`のブランチを指定する。
1. `liferay-portal-ee`のリポジトリを`github.dev`などで開く。
1. Macなら`Cmd+P`キーで、ファイル検索を表示し、`SAML`と検索する。
1. `SamlWebUpgrade.java`というようなファイル名が見つかるので、開いて、プロジェクトビューでディレクトリ構造を確認

あとはWidgetの画面なら`SamlAdminPortlet`を確認、ビジネスロジックなら`saml-impl`を開き、ファイル構造を見ていく


### Liferayに表示されている画面からあたりをつける
Liferayのウィジェットはすべて、パッケージ名のクラス名がついた`div`タグで囲われているので、それを手がかりにする。

## ライブラリの依存関係
LiferayのOSGiバンドルは、`bnd.bnd`ファイルをベースに生成されます。この仕様などの詳細は[この辺の公式ドキュメント]()を見て貰えばわかりますが、ここでは依存関係に関してかいつまんで説明します。
### Liferayコード内の必要なライブラリ名を探す方法
ここには該当バンドル名が入ります。ウィジェットを自作している時に、必要なライブラリを探す時に、使います。例えば、掲示板のカスタマイズをしている時に、`MBThreadLocalService`を利用したいとしましょう。その場合
1. `liferay-portal`の中を`MBThreadLocalService`で検索。`MBThreadLocalService.java`が見つかる。
1. そのプロジェクト、`message-board-api`の直下にある`bnd.bnd`ファイルを開く
1. `Bundle-SymbolicName`の名前、この場合は`com.liferay.message.boards.api`が、`build.gradle`もしくは`pom.xml`ファイル内で指定するライブラリ名になります。具体的には、`build.gradle`だと、
    ```bash
    compileOnly group: "com.liferay.portal", name: "com.liferay.message.boards.api"
    ```
    のようになる。(バージョンはBOMから取得される。)
## Best Practiceの見つけ方
基本的に、Liferayのファイル構成、命名規則やコードの取り回しを真似する形から作るのがよい。いい参考になるのが
- Blogs (`blogs-service`,`blogs-web`など)
- Web Content (`journal-service`, `journal-web`など)

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
Liferayのコード、ファイル名などに規約があります。その辺を理解すると、コードの理解が素早くできるようになります。

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
# 情報の検索方法
## Github検索
## Liferayコアコードの参照
## Best Practiceの見つけ方
### React or Vue or Angularの場合
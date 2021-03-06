---
title: "Liferayとは？"
free: false
---

# Liferayとは？
Liferayは2000年にチーフ・ソフトウェア・アーキテクトであるBrian Chanによって、Javaベースの非営利団体向けのエンタープライズ・ポータルを起源とし、現在はB2BのECサイト機能、ログインユーザー解析機能を備えたDXP(デジタルエクスペリエンスプラットフォーム）として世界中で利用されています。

2021年現在、Liferayは世界中に22のオフィスを持ち、グローバルで1200名以上のメンバー、250社以上のパートナー企業、18万人のオープンソースコミュニティメンバーを擁しています。

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

# Liferayで何ができるの？
Liferay上では、ユーザーがログインし、それぞれのユーザーが権限に従って、情報の共有や検索、また製品の見積もり、売買などの取引を行うことができます。

ざっくり以下のような用途で使われています。
- さまざまなフォーマットの大量のコンテンツを、適切な権限管理で、簡単に共有したい
- 複数のディーラーさん、子会社さん、お客様の店舗、またその先のお客さんとのデータのやりとりや売買取引を、オンライン上で完結させたい

# Liferayのフィットする用途
以下でもう少し詳細に説明してみます。

### 凡例
| ステータス | x | △ | ○ | ◎ |
|:--|:--|:--|:--|:--|
| 意味 | 向かない | カスタマイズで可能 | 標準機能のみで可能 | 標準機能で、更にわずかな設定で統合可能 |

## 向いてる用途
| 用途 | 理由 | ステータス |
|:--|:--|:--|
| ユーザー数が数千名以上で、ユーザー課金でないプラットフォームを探している。<br /><br />**例：社内ポータル** | 今グループウェアなどを利用している場合は、Liferayのライセンス価格がコスト的に低く抑えられる可能性がある（ユーザー課金ではなく、サーバーのインスタンス課金のため） | ◎ |
| 複数サイトを容易に権限管理したい <br /><br />**例：官公庁ポータル**| Liferayでは官公庁で利用されている例も多く、時には数千サイトを統合された権限管理下で管理しているというケースもある | ◎ |
| 複数の子会社、ディーラー間で権限管理を効かせながら、情報共有をしたい <br /><br />**例：ディーラーポータル**| 社内ポータル、ディーラーポータルで利用するケースは国内外で一番Liferayが使われているユースケース。|◎ |
| マーケットプレースを作りたい <br /><br />**例：B2B2Cコマース**| Liferayには**Commerce**機能が付属しており、それを利用すればテナント型マーケットプレースや、複数ブランドを一括管理しつつ、それぞれ別のECサイトがある、といういわゆる**B2B2C**のECサイトが構築可能。| ○ |
| 商品カタログサイトを構築したい <br /><br />**例：B2Bコマース**| **Liferay Commerce**には標準で商品カタログ機能、商品表(BOM)機能がある。SAP ERP/B1などのERPとの統合も実績があり、可能。| ○ |
| 大量の子会社があり、それぞれで利用しているシステムを一括管理するフロントエンドとして利用したい。<br /><br />**例：統合ポータル** | 複数社のシステムを統合する、というケースも多く、SAMLやLDAP、OpenID Connectなど、多種多様な認証機構のコネクタを標準で装備している。|◎ |
| 大量の子会社のデータを一括検索、管理したい <br /><br />**例：統合ポータル、社内ポータル、ディーラーポータル**| Liferayの検索はElasticsearchを利用しており、Elastic Workspaceなどを利用して、Office 365, Google Workspace, Box, Dropbox, Salesforceなどと、画面上の設定からLiferayと連携可能 |○ |

## 強みのある機能
| 機能 | 理由 | ステータス |
|:--|:--|:--|
| SSO | OAuth2.0, OpenID Connect, SAML2.0、LDAP、Kerberos, CASなどに対応。|◎ |
| 外部システムとのインテグレーション | Liferay自体のコンポーネントがREST, GraphQL, SOAPなどの連携をサポートしている。 | ◎ |
| マルチテナント | 1つLiferay上で、複数URLを複数仮想インスタンスに対して割り当て可能。各仮想インスタンスに対して、別々のSSO設定に対応 |◎ |
| 多言語対応 | 43言語に対応。それ以外の言語もLanguage.propertiesを作成すれば対応可能　|◎ |
|権限管理 | ページ、コンテンツ、ウィジェット単位、１テキストフィールド単位までロールとパーミッションで管理可能 |◎ |
| 大規模CMS | バージョン管理、承認ワークフロー、複数コンテンツをグルーピングしての公開機能、検索など、多数のコンテンツ作成チーム、承認フローと、大企業での利用で想定されるような複雑なリリースフローに対応している。| ◎ |
| 大規模サイトにも耐えられる検索機能 | Elasticserchを利用し、数百万、数千万コンテンツの検索にも耐えられる基盤を持つ。多言語での検索にも対応し、表示画面言語毎に、Analyzer, Indexerを細かく設定、任意のElastic Pluginを割り当てることも可能。DXP Cloud, Elastic Cloudなどマネージドサービスを利用することで、検索インフラ管理の手間を大きく削減することも可能。| ◎ |
| ノーコード(No-code)でのカスタムアプリ作成 | **Liferay Object**を利用してGUI上からカスタムデータベース・テーブルスキーマを作成可能。複数のテーブルのリレーションも作成可能。View部分は*Liferay Form*を利用して、データ編集ページを作成可能、またテンプレートを利用して、その見栄えを変更することも容易である。|◎ |
|検索チューニングの容易さ | **Blueprint**という検索チューニング機能により、GUI上で検索結果をリアルタイムに確認しながらのElasticsearchパラメータチューニングが可能。| ◎ |

## Liferayが向いてないかもしれないケース
Liferayでできないこともないけれど、正直他のツールやアプリの方が使い勝手がいいかも、というのもあげてみました。

:::message
金額面などに触れている部分は、有料版を利用されて、サポートもちゃんと受けたい、という方の場合にのみ当てはまります。無料版をご利用になって、自らサーバーなどの環境を構築して、維持できるという場合はこの限りではありません。
:::


| 用途 | 代替アプリ | 理由 | 
|:--|:--|:--|
| 単純なCMS | Wordpress, STUDIO, Webflow | Liferayは企業などでの数万点などにわたるプロダクトカタログの管理や、公開、編集や、１つの製品に紐づく複数のマニュアル管理など、複雑な用途で力を発揮します。単純にブログのような感じで公開するのみが要件であれば、Liferayでももちろん実現可能ですが、コンテンツ作成後の管理、維持の容易さを考えると、これらツールを使ったほうが効率的＋素早くサイトを作成できるかもしれません。 |
| マーケティングオートメーション重視なサイトの運営 | SiteCore, Adobe Experience Cloud, Salesforce | キャンペーンやナーチャリング自動化や顧客分析、という部分はLiferayでも外部サービスと連携して可能ですが、Liferayだとカスタマイズが必要になります。マーケティング用途重視で、メインとなるプラットフォームとの高い親和性を求めるのであれば、これらプラットフォームも値段的には高くなりますが、検討してみる価値はあるかもしれません。| 
| 会社は数十名規模、すぐさま問題を解決したい | Kintone, Garoon | Kintoneなどは特定の問題を現場中心ですぐさま解決していく、というのに向いていますし、これらツールも使いやすく、安く始められると思います。利用ユーザー数が千名などの規模になってくると、グループウェアさんのユーザー課金体系と比べて、利用可能なLiferayの機能と、サーバーライセンス金額体系や、PaaSなどと比較して、Liferayの利用にメリットがでてくるかもしれません。|
|単純なECサイト | Shopify, Stores, Base | LiferayのCommerce機能はB2B、B2B2C、B2Cでも大規模(商品点数数千、数万という規模、複数ブランドアイテムを複数ブランドサイトに分けて、1箇所で管理、など)なケースで利用する場合に力を発揮します。小さい規模でのECサイトももちろん構築可能ですが、であれば代替アプリにあげられたサービスを利用された方が素早くサイトを作成し、簡単に維持できます。|
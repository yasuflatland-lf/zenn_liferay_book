---
title: "Liferayの歩き方 (TBD)"
type: "tech"
topics: ["liferay", "java", "portal", "osgi"]
published: true
---

# はじめに
Liferayはオープンソースで、コードが公開されていますが、コードベースが巨大なため、コードを見ていくにもその構成を知っておいた方が、地図があってランドマークがわかると街が歩きやすいのと同様、コードを追いやすくなるかと思います。

この章ではLiferayの歩き方と称して、Liferayのコードベースの見方を紹介します。
# Liferayの種類
Liferayにはオープンソースバージョン(CE)と有償版(DXP)の２つが存在します。
| 種類| リポジトリ |
|:-- |:--|
| Liferay CE | [https://github.com/liferay/liferay-portal](https://github.com/liferay/liferay-portal) |
| Liferay DXP | [https://github.com/liferay/liferay-portal-ee](https://github.com/liferay/liferay-portal-ee) |


有償版(DXP)には
| 種類| 説明 |
|:-- |:--|
| SAML 2.0 Plugin | SAML2.0形式でADFSやOktaなどと連携が可能。Liferayは通常SPとして利用する。SPとして複数IdPとの接続(IdP Initiative / SP Initiativeの両方)もできる。Liferay自体もIdP/SP両方で設定可能だが、バージョンアップやセキュリティアップデートなどの実運用を考えて、**LiferayをIdPとして利用するのは推奨しない。(開発時のテスト用などならOK)** |
| 多要素認証 | Google Authenticator を利用した多要素に対応 |
| Liferay Cluster | 本番環境にはほぼ必須のLiferayのEhcacheクラスタリング機能 |
| フィックスパック | 2,3ヶ月に一回、バグ修正のパッチがリリースされる |
| サポート | 問い合わせ数無制限のサポートチケットが利用できる |

などが付属してきます。興味がある方は[ホワイトペーパー](https://www.liferay.com/en-AU/resources/product-info/Comparativo+de+Produtos+-+Liferay+DXP+7.3+vs+Liferay+Portal+CE+7.3)を参考ください。

# 暗黙のコード規約
Liferayのコードにはいくつかのルールがあります。その辺を理解すると、コードの理解が素早くできるようになります。

## ドメインモデル
Liferayは3層アーキテクチャーで構成され、

## サービスビルダーサービス・ウィジェット

# コードの追い方
# 情報の検索方法
## Github検索
## Liferayコアコードの参照
## Best Practiceの見つけ方
### React or Vue or Angularの場合
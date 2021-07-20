---
title: "Liferayを使う準備"
free: false
---

# Liferay 7 / DXPのインストール(Tomcatバンドル)
## Dockerのインストール
Dockerのインストールは[Macはこちらから](https://docs.docker.com/docker-for-mac/install/)、[Windowsはこちらを参考に](https://docs.docker.com/docker-for-windows/install/)インストールしてください。

## データベースの準備
データベースはMySQL5.7を利用します。すでに使用している方はそのサーバーを利用してください。データベースの環境が何もない方は、拙作の[Docker Composeをご利用](https://github.com/yasuflatland-lf/mysql-compose)ください。Gitをインストールしていない方は、リポジトリの`Code`ボタンをクリック、コードをダウンロードしてください。

リポジトリのルートディレクトリで
```
docker-compose up --build
```
とすると起動します。自動的に73ee, 73ceのデータベースが作成されています。データベースはutf8mb4で作成する必要があります。（でないと、🍣と🍺や、旧字体の漢字などが正しくデータベースに保存されません。)

`Squel-ace`や`MySQL Workbench`などを利用して`localhost`にユーザー名`root`、パスワード`password`でアクセスできるはずです。デフォルトで`73ee`というデータベースが作成されているはずです。

任意の名前でデータベースを作成する場合は、Data Baseencordingを`utf8mb4`、Database Collationを`utf8mb4_unicode_ci`を選択して作成してください。

## 検索エンジン
検索エンジンは、組み込みのElasticsearchを利用しますので、ここでは設定は必要ありません。（プロダクションの場合は、別途Elasticsearchクラスタを立てる必要があります。)

## JDKのインストール
ここではAdoptOpenJDKを利用する場合の説明をします。
1. [AdoptOpenJDKのページ](https://adoptopenjdk.net/installation.html)から該当のJDKをインストール。(ここでは`OpenJDK 8 (LTS)`を選択します）
2. 次に`Platform`を選択。`Mac`なら`macOS x64 jdk`、Windows 10なら`Windows x86 jdk`, Windows Serverなら`Windows x64 jdk`を選択。
3. その下に表示されるインストール手順に従ってインストール。

## JDKのパスを通す
### Windows
- 環境設定を開き、以下のシステム環境変数を設定する(以下はJDK 8の例)。項目がなければ新規に作成する。

|環境変数 | 設定値（サンプル) |
|:-------|:-----|
| JAVA_HOME | C:¥Program Files¥java¥jdk1.8.0_152 |
|JRE_HOME | C:¥Program Files¥java¥jdk1.8.0_152 |
| Path | 多分すでに値が設定されているものがあると思うので、最後にコロン区切りで```C:¥Program Files¥java¥jdk1.8.0_152¥bin``` |

## Mac
利用しているシェルの設定ファイルに以下を記述。(Mac標準のzshで、`.zshrc`を利用していることを想定。JDK8を利用している場合は`-v 1.8`を利用、JDK11の場合は`-v 11`の方を利用し、一方をコメントアウトする。以下はJDK8を利用している場合。
```
# JDK11の場合は以下のコメントを外し、1.8の方をコメントアウト
# export JAVA_HOME=`/usr/libexec/java_home -v 11`

# JDK8の場合は以下のコメントを外し、11の方をコメントアウト
export JAVA_HOME=`/usr/libexec/java_home -v 1.8`
```

## Liferay DXP/CEのダウンロード
- [Liferay CE版の最新版ダウンロード](https://sourceforge.net/projects/lportal/files/Liferay%20Portal/)はこちら。
- [Liferay DXPのダウンロード](https://help.liferay.com/hc/en-us/articles/360029031491-Installing-Liferay-DXP-on-Tomcat)

## インストール
[公式インストールガイド](https://help.liferay.com/hc/en-us/articles/360029031491-Installing-Liferay-DXP-on-Tomcat)はこちら。

### Windows
1. ダウンロードしたバンドル(Tomcatバンドル版をダウンロードすることを想定しています。）をCドライブ直下などに解凍する。ユーザーホーム（デスクトップなど）で解凍すると、パスが長すぎて解凍できない、というようなエラーが発生する可能性があります。
1. 解凍したルートディレクトリを```${liferay_home}```とした場合、そこに`portal-ext.properties`という名前でファイルを作成し、以下を参考にの内容を保存します。`lrportal`の部分は、ご自身のデータベース名に変更してください。
   ```
   jdbc.default.driverClassName=com.mysql.cj.jdbc.Driver
   jdbc.default.url=jdbc:mysql://mysql/lrportal?connectionCollation=utf8mb4_bin&dontTrackOpenResources=true&holdResultsOpenOverStatementClose=true&serverTimezone=GMT&useFastDateParsing=false&useUnicode=true&characterEncoding=utf8
   jdbc.default.username=root
   jdbc.default.password=password
   ```
3. `${liferay_home}¥tomcat-8.0.32¥bin`に移動します。(Tomcatのバージョンは新しくなっている可能性があります)
4. メモリに余裕がある場合、`setenv.bat`の`CATALINA_OPTS`を以下のように変更します。
   ```
   CATALINA_OPTS="$CATALINA_OPTS -Dfile.encoding=UTF-8 -Djava.locale.providers=JRE,COMPAT,CLDR -Djava.net.preferIPv4Stack=true -Duser.timezone=GMT -Xms4G -Xmx4G -XX:MaxNewSize=1536m -XX:MaxMetaspaceSize=768m -XX:MetaspaceSize=768m -XX:NewSize=1536m -XX:SurvivorRatio=7"
1. ```startup.bat```をダブルクリックすると、Liferayが起動します。

### Mac
1. ダウンロードしたバンドル(Tomcatバンドル版をダウンロードすることを想定しています。）を解凍する。
1. 解凍したルートディレクトリを```${liferay_home}```とした場合、そこに`portal-ext.properties`という名前でファイルを作成し、以下を参考にの内容を保存します。`lrportal`の部分は、ご自身のデータベース名に変更してください。
   ```
   jdbc.default.driverClassName=com.mysql.cj.jdbc.Driver
   jdbc.default.url=jdbc:mysql://mysql/lrportal?connectionCollation=utf8mb4_bin&dontTrackOpenResources=true&holdResultsOpenOverStatementClose=true&serverTimezone=GMT&useFastDateParsing=false&useUnicode=true&characterEncoding=utf8
   jdbc.default.username=root
   jdbc.default.password=password
   ```
3. ```${liferay_home}/tomcat-8.0.32/bin```に移動します。(Tomcatのバージョンは新しくなっている可能性があります)
4. メモリに余裕がある場合、`setenv.bat`の`CATALINA_OPTS`を以下のように変更します。
   ```
   CATALINA_OPTS="$CATALINA_OPTS -Dfile.encoding=UTF-8 -Djava.locale.providers=JRE,COMPAT,CLDR -Djava.net.preferIPv4Stack=true -Duser.timezone=GMT -Xms4G -Xmx4G -XX:MaxNewSize=1536m -XX:MaxMetaspaceSize=768m -XX:MetaspaceSize=768m -XX:NewSize=1536m -XX:SurvivorRatio=7"
3. `./startup.sh run`を実行すると、Liferayが起動します。

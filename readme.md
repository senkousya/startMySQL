# 🔰MySQL CommunityEditionをさわってみる(windows)

## 🔰MySQLとはなんぞや

MySQLはオープンソースなRDBMS(relational database management system)。1995年に公開されて、そこから色々とあり現在はOracleが管理している。

MySQLには商用ライセンスとCommunity Edition(GPL)が存在しており、今回はお遊びで触るのでCommunityEditionをさわっていく。

※MySQLのGPLライセンス周りって調べはじめると色々と奥深い世界が広がっているようで。どういう場合に商用ライセンスが必要なのかよくわかんない……

## 🔰MySQL CommunityEditionをダウンロードしてみる。

[MySQL](https://www.mysql.com/jp/)

ダウンロードページの下の方にCommunityEditionがあるのでダウンロードページヘ。

![](image/download.MySQLCE.step001.png)

▶MySQL Community Server (GPL)を選択。  
![](image/download.MySQLCE.step002.png)

▶msiインストーラは別ページなようなのでダウロードページへ移動  
![](image/download.MySQLCE.step003.png)

オンラインでインストールするかオフラインでインストールするかによってインストーラが違う様子。
とりえあずオフライン用を選択してダウンロード。

▶DOWNLOAD  
![](image/download.MySQLCE.step004.png)

なんかアカウント登録してって言われますが、スキップできるのでスキップ！

▶No thanks,just start my download.  
![](image/download.MySQLCE.step005.png)

**mysql-installer-community-5.7.21.0.msi**がダウンロードできました。

▶ダウンロードしたファイル  
![](image/msiFile.png)

## 🔰MySQL CommunityEditionをインストールしてみる。

▶ダウンロードしたインストーラを実行  
![](image/install.MySQLCE.step001.png)

インストールするグループを選択する。今回はとりあえずcustomでインストール物を自分で指定する。

▶Customを選択  
![](image/install.MySQLCE.step002.png)

左のパネルに一覧が出るので、インストールする製品を選択して右に移していく。

今回はとりあえず下記のように選択してみた。

- MySQL Server
- MySQL Workbentch(MySQLの管理ツール)
- connector/ODBCとconnector/C++とconnector/.NET

![](image/install.MySQLCE.step003.png)

ちなみにAdvanced Optionsを選択すると、インストール先とデータディレクトリを指定できたりします。

▶ディレクトリの指定  
![](image/install.MySQLCE.step004.png)

▶Execute  
![](image/install.MySQLCE.step005.png)

インストールが全部コンプリート！

▶complete  
![](image/install.MySQLCE.step006.png)

▶Next  
![](image/install.MySQLCE.step007.png)

なんかスタンドアロンかクラスタかの選択肢が……今回はスタンドアロンを選択

![](image/install.MySQLCE.step008.png)

特に変更する要素はなかったのでそのままNext。

![](image/install.MySQLCE.step009.png)

Rootアカウントのパスワードを設定します。

![](image/install.MySQLCE.step010.png)

MySQLのservice名と動かすユーザとか。特に変更する要素はなかったのでそのまま。

![](image/install.MySQLCE.step011.png)

![](image/install.MySQLCE.step012.png)

▶Execute  
![](image/install.MySQLCE.step013.png)

▶Finish  
![](image/install.MySQLCE.step014.png)

▶Next  
![](image/install.MySQLCE.step015.png)

▶Next  
![](image/install.MySQLCE.step016.png)

先程の画面で`Start MySQL Workbench after Setup`のチェックを外し忘れたので`MySQL Workbench`が起動しました……

▶MySQL Workbench
![](image/install.MySQLCE.step017.png)

## mysql.exeでバージョンを確認してみる。

インストールしたディレクトリのbin(今回は`C:\Program Files\MySQL\MySQL Server 5.7\bin`)に`mysql.exe`があるので引数`--version`を渡して実行するとバージョンが表示される。

その後、`get-service`でMySQLのサービス状態を確認してみる。（今回インストール時にサービス名を`MYSQL57`でデフォルト指定したやつ)

![](image/mysql57version.png)

## MySQL Command Line Clinetを利用してmysqlへ接続してみる

MySQLCommandLineClientのショートカットから起動。

![](image/mysqlCommandLineClient.png)

ちなみに、ショートカット中身をみるとmysql.exeにiniファイルを食べたせて、ユーザはrootを利用するパラメータを渡している様子。

`"C:\Program Files\MySQL\MySQL Server 5.7\bin\mysql.exe" "--defaults-file=C:\ProgramData\MySQL\MySQL Server 5.7\my.ini" "-uroot" "-p"`

![](image/mysqlCommandLineClientShortcut.png)

インストール時に指定したrootのパスワードを入力してログイン

![](image/mysqlCommandLineClient.step001.png)

`\c`でステートメントのクリアができるよとか書いてますが。Windows環境だとクリアできないみたいです？

[Bug #58680 Windows Clear Screen Command](https://bugs.mysql.com/bug.php?id=58680)

## Databaseをcreateしたり、tableをcreateしてみる

今回、とりあえずhelloworldというdbを作成。そこにsample001というテーブルを作成してみます。

### どんなDatabaseがあるかみてみる

`show databases;`でどんなdatabaseがあるかみてみます。

![](image/showDatabase.png)

- infomation_schema
- mysql
- performance_schema
- sys

なるdatabaseが存在するようです。

### databaseのcreateと確認

`create database helloworld;`でdatabaseをcreateします。

![](image/createHelloworldStep001.png)

`show databases;`でdatabaseに`helloworld`が追加された事を確認。

![](image/createHelloworldStep002.png)

### database helloworldにtableをcreateしてみる

`use helloworld;`で先程作成したhelloworld databaseへ接続。

`show tables;`でdatabaseに登録されているテーブルの一覧を確認。何も作ってないのでEmpty。

![](image/createHelloworldStep003.png)

tablesがなにもないので、create tableでsample001というテーブルを作成してみます。

```sql
create table sample001
( id int , username varchar(255) );
```

![](image/createHelloworldStep004.png)

`show tables;`コマンドでtable sample001が登録された事が確認できる。

![](image/createHelloworldStep005.png)

`describe`コマンドでtable sample001の構造を確認してみる。

`describe sample001;`

![](image/createHelloworldStep006.png)

### insertコマンドでレコードをinsertしてみる

`insert`コマンドでレコードを登録してみます。

`insert into sample001 (id , username) values (1, 'sample');`

![](image/createHelloworldStep007.png)

### selectコマンドで登録したデータを表示してみる

`select`コマンドで登録したデータを表示してみます。

`select * from sample001;`

![](/image/createHelloworldStep008.png)

さきほどinsertしたデータが表示されます。

## 🔰総評

インストールしたり、データベースやテーブルつくるのはさっくりできたけど。ライセンス周りの話はやっぱ難しい。

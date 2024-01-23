# サーバーを移行する際にコピーが必要な設定情報

## 各種設定情報のインポート方法

### 0.iris.cpf

  他環境からコピーした iris.cpf を別環境に移行する場合、設定値やパスを移行先環境にあわせて設定した後、以下のコマンドで設定ファイルに問題がないか検証を行います。
　%SYS>Do ##Class(Config.CPF).Validate("C:¥...¥iris.cpf")

  移行先のIRISのバージョンが異なる場合、設定ファイルを移行先バージョンに変換します
　%SYS>Write ##Class(Config.CPF).Convert("C:¥...¥iris.cpf")

※CacheからIRISへ移行する場合は、メインの構成ファイルのエントリの一部や、データベース名、既定値など大きく異なります。
　構成ファイルに問題があるとIRISの起動ができなることがありますので、変更設定は慎重に行うようにしてください。
　IRISの構成ファイルパラメータについては、以下のドキュメントを参照してください。
　https://docs.intersystems.com/irislatestj/csp/docbook/Doc.View.cls?KEY=RACS

### 1. SQLゲートウェイ設定情報

データ・エクスポートウィザードでエクスポートしたファイル(例：dataexport.txt)を、管理ポータルでインポートします。

システムエクスプローラ > SQL > ウィザード > データ・インポートウィザード
  インポートファイルのパスおよび名前：<エクスポートファイルを置いているパス>¥dataexport.txt
  インポートするネームスペース：%SYS
  インポートするスキーマ名：%Library
  インポートするテーブル名：sys_SQLConnection

  以降はデフォルトで [完了]

### 2. WEBゲートウェイ(CSPゲートウェイ)設定情報

csp.iniファイル
※Webゲートウェイインストールフォルダ(例：C:¥inetpub¥CSPGateway)にコピーします
※プライベートWebサーバ(PWS)の場合は、インストールディレクトリの下のCSP下のbinディレクトリにコピーします。

### 3. 優先接続サーバー設定

Windowsのみに存在する情報です。
Windowsのレジストリーに保存される情報です。
レジストリー情報をエクスポートし、他システムにインポートすることは推奨されていません。
お手数ですが、移行先で再設定してください。

### 4. セキュリティ設定

ターミナルでログインし、%SYSネームスペースに移動します。

>DO ^SECURITY

Option 12を選んで、6) Import All Security settings を実行します。

Import from file name SecurityExport.xml => 

こちらで、移行元環境で事前に 5)のExport All Security settings にてエクスポートしたファイルパスを指定します。

### 5. タスク設定

タスク設定のインポートは以下をご参照ください。

[タスク設定のインポート](https://faq.intersystems.co.jp/csp/faq/result.csp?DocNo=412)

## 各種設定情報のエクスポート方法

### 1. SQLゲートウェイ設定情報

テーブル：%Library.sys_SQLConnection

こちらにある情報を管理ポータルでエクスポートします。

システムエクスプローラ > SQL > ウィザード > データ・エクスポートウィザード
  エクスポート先のパスおよび名前を指定：<エクスポートファイルパス>    ※デフォルトで <IRISインストールディレクトリ>\mgr\dataexport.txt にエクスポートされます。
  エクスポートするネームスペース：%SYS
  エクスポートするスキーマ名：%Library
  エクスポートするテーブル名：sys_SQLConnection

### 2. WEBゲートウェイ(CSPゲートウェイ)設定情報

csp.iniファイル

※Webゲートウェイインストールフォルダ(例：C:\inetpub\CSPGateway)に存在します
※プライベートWebサーバ(PWS)の場合は、インストールディレクトリの下のCSP下のbinディレクトリに存在します。



### 3. 優先接続サーバー設定

Windowsのみに存在する情報です。
Windowsのレジストリーに保存される情報です。
レジストリー情報をエクスポートし、他システムにインポートすることは推奨されていません。
お手数ですが、移行先で再設定してください。

### 4. セキュリティ設定

ターミナルでログインし、%SYSネームスペースに移動します。

>DO ^SECURITY

Option 12を選んで、5)のExport All Security settingsを実行します。


### 5. タスク設定

タスク設定のエクスポートは以下をご参照ください。

[タスク設定のエクスポート](https://faq.intersystems.co.jp/csp/faq/result.csp?DocNo=412)

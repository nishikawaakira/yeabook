# Splunk

Splunk サーバの構築と Splunk Universal Forwarder のインストール
-------------

ログの確認および検知ルールの検討を容易にするために Splunk 環境を構築します。
検証環境に Splunk Universal Forwarder をインストールし、イベントログを Splunk サーバへ集約します。
本章では Splunk 環境の構築手順について記載します。

ログ転送の構成
-------------

今回は、検証環境にそれぞれ Splunk Universal Forwarder をインストールしイベントログを Splunk サーバへ転送します。

![Splunk-3](images/Splunk/3.png)

インストーラ
-------------

以下のサイトからインストーラのダウンロードを行います。

**Splunk Enterprise のダウンロード**

https://www.splunk.com/en_us/download/splunk-enterprise.html


**Splunk Universal Forwarder のダウンロード**

https://www.splunk.com/en_us/download/universal-forwarder.html

2021年4月3日時点では以下のバージョンとなります。
- Splunk Enterprise 8.1.3
- Splunk Universal Forwarder 8.1.3

![Splunk-1](images/Splunk/1.png)

![Splunk-2](images/Splunk/2.png)

Splunk のインストール
-------------

Splunk をインストールするサーバ上で、インストーラを実行します。
その後は以下の手順に従ってインストールを完了させます。

・ライセンスに同意し「Customize Options」を押下します。

![Splunk-4](images/Splunk/4.png)

・そのまま「Next」で進めます。

![Splunk-5](images/Splunk/5.png)

・管理者の Username と Password を設定します。

![Splunk-6](images/Splunk/6.png)

・「Install」を押下しインストールを開始します。インストールが完了した後に「Finish」を押下しインストールを完了させます。

![Splunk-7](images/Splunk/7.png)

・インストール後、ブラウザにて「127.0.0.1:8000」にアクセスを行い、インストール時に設定したユーザ名とパスワードでログインを行います。

![Splunk-8](images/Splunk/8.png)

・上部のメニュにある「設定」を押下します。

![Splunk-9](images/Splunk/9.png)

・「データ -> 転送と受信」を選択します。

![Splunk-10](images/Splunk/10.png)

・データの受信で「新規作成」を押下します。

![Splunk-11](images/Splunk/11.png)

・受信の設定にて Listen するポート番号を設定します。ここではデフォルトのポート番号である 9997 を設定しています。

![Splunk-12](images/Splunk/12.png)

・Windows Firewall で Listen ポートである 9997 番ポートを解放します。規則の種類で「ポート」を選択します。

![Splunk-13](images/Splunk/13.png)

・「TCP」の特定のローカルポートとして 9997 を設定します。

![Splunk-14](images/Splunk/14.png)

・ログ受信用のルールが設定されたことを確認します。ここではルール名を「Splunk Universal Forwarder」としています。

![Splunk-15](images/Splunk/15.png)


Splunk APP の追加
-------------

APP を追加するためには、APP のサーチから APP の名称で検索することが可能です。

![Splunk-16](images/Splunk/16.png)

本検証環境では、以下の APP をインストールします。

- Splunk Add-on for Microsoft Windows
- Splunk Add-On for Microsoft Sysmon
- Force Directed App For Splunk

Splunk Universal Forwarder のインストール
-------------

ログを取得したい端末にて Splunk Universal Forwarder をインストールします。
標準機能でのイベントログの転送設定後、拡張ログである Powershell ログ、および Sysmon を転送するための設定を追加していきます。

・Splunk Universal Forwarder のインストーラを実行します。ライセンスに同意し On-premise を選択したのち「Customize Options」を押下します。

![Splunk-17](images/Splunk/17.png)

・そのまま「Next」で進めます。

![Splunk-18](images/Splunk/18.png)

・証明書は設定せずに進めます。

![Splunk-19](images/Splunk/19.png)

・ローカルシステムでのインストールを選択します。

![Splunk-20](images/Splunk/20.png)

・転送するログについては、「Application Logs」「Security Logs」「System Logs」を選択します。

![Splunk-21](images/Splunk/21.png)

・管理者の Username と Password を設定します。

![Splunk-22](images/Splunk/22.png)

・Deployment Server は構築しないためブランクで進めます。

![Splunk-23](images/Splunk/23.png)

・構築した Splunk サーバの IP アドレスとポート番号を設定します。

![Splunk-24](images/Splunk/24.png)

・その後、「Install」を押下することでインストールが開始されます。

![Splunk-25](images/Splunk/25.png)

拡張ログの転送設定
-------------

追加の設定として、以下の拡張ログを転送する設定を行います。

- Microsoft-Windows-PowerShell/Operational
- Microsoft-Windows-Sysmon/Operational
- Microsoft-Windows-Sysmon/Operational

Splunk Universal Forwarder の設定ファイルである inputs.conf ファイルで追加の転送設定が可能です。

![Splunk-26](images/Splunk/26.png)

C:\Program Files\SplunkUniversalForwarder\etc\system\local ディレクトリにある inputs.conf ファイルに以下の内容を追記します。

```
[default]
host = user1-PC※ホスト名を入れる

[WinEventLog://Microsoft-Windows-PowerShell/Operational]
disabled = false
renderXml = true

[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = false
renderXml = true

[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = false
renderXml = true
```

inputs.conf ファイルの設定を反映させるために、サービスより Splunk Forwarder Service を再起動します。

![Splunk-27](images/Splunk/27.png)

サービスの再起動後から、追加のログの転送が開始されます。
# Splunk

Splunk サーバの構築と Splunk Universal Forwarder のインストール
-------------

ログの確認および検知ルールの検討を容易にするために Splunk 環境を構築します。
検証環境に Splunk Universal Forwarder をインストールし、イベントログを Splunk サーバへ集約します。
本章では Splunk 環境の構築手順について記載します。

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

Splunkサーバの構築
-------------

今回は、検証環境にそれぞれ Splunk Universal Forwarder をインストールします。

![Splunk-3](images/Splunk/3.png)

Splunk をインストールするサーバ上で、インストーラを実行します。
その後は以下の手順に従ってインストールを完了させます。

・ライセンスに同意し「Customize Options」を押下する。

![Splunk-4](images/Splunk/4.png)
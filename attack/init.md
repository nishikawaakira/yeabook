# 攻撃検証　はじめ

フォレンジックツール「yea」の検知ルール作成のために攻撃手法の検証、ログ出力の調査を行います。

本攻撃検証は、ベースの検証環境（YEAラボ）を用いて検証を行っています。

YEAラボに関するドキュメント
-------------
- AD Baseline Lab Tool


環境設定に関するドキュメント
-------------
- ロギング拡張
- AppLocker GPO to disable powershell


攻撃検証に関するドキュメント
-------------
現在検証中の攻撃手法は以下の通りです。（2021.03.10時点）

- [Kerberoasting](attack/kerberoasting.md)
- ASREP Roasting
- Powershell Downgrade Attacks
- Powershell without Powershell.exe
- Password Spray
- wsreset (LoLBAS)

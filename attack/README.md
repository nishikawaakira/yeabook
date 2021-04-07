# 攻撃検証

本章では攻撃手法の検証、ログ出力の調査を行います。

本攻撃検証は、ベースの検証環境（YEAラボ）を用いて検証を行っています。

環境設定
-------------
- [Enable Logging - Powershell](lab/logging-powershell.md)
- [Enable Logging - Sysmon](lab/logging-sysmon.md)
- [AppLocker GPO to disable powershell.exe](lab/AppLocker_GPO_for_PS.md)

攻撃検証
-------------
現在検証中の攻撃手法は以下の通りです。（2021.03.10時点）

- [Kerberoasting](kerberoasting.md)
- ASREP Roasting
- Powershell Downgrade Attacks
- Powershell without Powershell.exe
- Password Spray
- wsreset (LoLBAS)


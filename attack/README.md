# 攻撃検証

本章では攻撃手法の検証、ログ出力の調査を行います。

環境設定
-------------
- [Enable Logging - Powershell](lab/logging-powershell.md)
- [Enable Logging - Sysmon](lab/logging-sysmon.md)
- [AppLocker GPO to disable powershell.exe](lab/AppLocker_GPO_for_PS.md)

攻撃検証
-------------
現在検証中の攻撃手法は以下の通りです。（2021.04.07時点）

**InitialAccess**
- [T1566.001 - Phishing Spearphishing Attachment](InitialAccess/T1566.001.md)

**DefenseEvasion**
- [T1218.004 - InstallUtil](DefenseEvasion/T1218.004.md)

**CredentialAccess**
- [T1110.003 - Password Spraying](CredentialAccess/T1110.003.md)
- [T1558.003 - Kerberoasting](CredentialAccess/T1558.003.md)
# ルールファイルの書き方


```
title: PowerShell Execution Pipeline # ルールのタイトル
description: hogehoge                # ルールの説明
enabled: true                        # ルールの有効（true） : 無効（false）
author: Yea                          # 作成者
logsource: 
    product: windows                 # ここは基本的にwindowsでOK
detection:
    selection:                       # 以下に検知条件を書いていく
        # 必須
        # どのイベントログファイルから検知を行うか
        # Security
        # System
        # Application
        # Microsoft-Windows-PowerShell/Operational
        # などが入る
        Channel: Microsoft-Windows-PowerShell/Operational

        # 任意
        EventID: 4103

        # OR条件での検索を行う場合は下記のように書く
        ContextInfo:
            - Host Application
            - ホスト アプリケーション
    # condition: selection
falsepositives:
    - unknown   # 今（2021/2/26時点）は使っていない
level: medium   # 重要度 low, medium, high のいずれかが入る
# 出力したい内容。
# 「%」で区切った英字はconfig/eventkey_alias.txtに記載したエイリアスのイベントログ内の値が記載される
output: 'command=%CommandLine%' 
creation_date: 2020/11/8    # 作成日
updated_date: 2020/11/8     # 更新日
```

[eventkey_alias.txtについて](eventkey_alias.md)

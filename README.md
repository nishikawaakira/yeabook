# Introduction
Yamato Event Analyzer - Yea!

## 使い方

```text
./yamato_event_analyzer -f [イベントファイル]
```

## オプション

##### ■検知オプション

ファイル単位 Windowsイベントログファイルを指定する  
-f --filepath=[FILEPATH]

ディレクトリ単位 Windowsイベントログが入っているディレクトリを指定する  
-d --directory=[DIRECTORY]  


##### ■CSV出力
--csv-timeline=[CSV_TIMELINE]

##### ■ログの時間を設定するオプション
RFC 2822 formatでの出力。 例: Mon, 07 Aug 2006 12:34:56 -0600  
--rfc-2822  

UTC formatでの出力。デフォルトは ローカルタイム  
-u --utc 



# ソースコードの流れ

イベントの解析を行う場合は、
src/main.rsから detections/detection.rs　の start 関数がコールされる

## start関数の流れ
1.ルールファイルをパース  
2.イベントファイルをjsonに変換  
3.ルール毎に一致するイベントログがあるかどうかを検知  
4.検知できた場合にはメッセージ表示配列変数にデータを格納  

という流れになる。
下記では、それぞれをもう少し詳細に記載する
  
## ルールファイルをパース
1.ルールディレクトリ内のルールファイル一覧を読み込む
2.ルールファイルをパースする detections/rule.rs の中のparse_rule関数がコールされてパース処理が行われる
3....

## イベントファイルをjsonに変換
イベントログのパーサーには下記を使用している
https://github.com/omerbenamram/evtx

現状のパーサーのoptionに関しては下記を参考
https://github.com/YamatoSecurity/YamatoEventAnalyzer/issues/57


## ルール毎に一致するイベントログがあるかどうかを検知
detections/rule.rsの中のselect関数がコールされて検知ロジックが始まる

## 検知できた場合にはメッセージ表示配列変数にデータを格納
detections/print.rsの中のinsert関数がコールされてデータが格納される。
最終的にmain.rsに戻りafter_fact.rsのafter_fact関数がコールされて、オプションによって標準出力やCSV出力などがされる。


## その他
・現在 detections/config.rs内に引数やeventkey_aliasの情報を持つSingletonの構造体を定義していて、
そこで設定情報を扱うようにしている

・detections/utils.rs には共通処理を書いている




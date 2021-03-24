# GitHub運用ルール

※以下の作業、全てプロジェクトのルートディレクトリ前提

##### 1.mainからfeature/xxx ブランチを作成する  <span>(xxxには機能名がはいる。機能名は、わかりやすいものを）</span>

```
$ git checkout -b feature/xxx ←古い  
```
or 
``` 
$ git switch -c feature/xxx     ←新しい(こっちに慣れるといいかも)
```


##### 2.機能を実装

##### 3.rustのformatに合わせる（おまじない）
```
$ cargo fmt --all
```

##### 4.変更内容をコミット対象にする
```
$ git add .
```

##### 5.ローカルリポジトリにコミットを行う
```
$ git commit -m “コミットコメント”
```

##### 6.ローカルリポジトリの内容をリモートリポジトリに反映させる
```
$ git push origin feature/xxx
```

##### 7.プルリクエストを西川宛に送る
(https://github.com/YamatoSecurity/YamatoEventAnalyzer にアクセスすると上部にプルリクするボタンが出現する、それをクリック)

まだ、作業中（まだレビューして欲しくない）の時は、タイトルの先頭にWIP:をつけましょう。外すのを、レビューお願いしますの合図にしましょう。

##### 8.西川がレビューしてフィードバックがあれば、対応。なければ、プルリクエストした人が責任を持ってmainにマージして終了。

マージ方法は「Squash and merge」を選択してください。  

**<font color="red">※注意事項
mainへは直接pushはしないでください。また、mainへのマージは必ず西川がプルリクをレビュー後、問題なければ実装者がmainへのマージをしてください。
オープンソースプロジェクトになった際には、これらは設定にてブロックをしますが、privateで開発している間はこの運用ルールの徹底をお願いいたします。
</font>**

## ■issueへの対応
feature/[機能の概要(英語)]#[issue番号] や fix/[修正の概要(英語)]#[issue番号]などブランチを切って対応してください。  
複数のissueに対応する場合はカンマ区切りで feature/[機能の概要(英語)]#[issue番号],#[issue番号]　や fix/[修正の概要(英語)]#[issue番号],#[issue番号]　という形でブランチを切って対応してください。  
issueへの対応する場合は、対応を行っていることを他の人に知らせるため、ブランチを切ったあと、すぐプルリクエスト を行ってください。

web上プルリクエスト を送る際、説明文に  
resolved #[issue番号]  
などのように、キーワードを使用してプルリクエストをIssueにリンクすることができます。キーワードを使用すると、マージされた時に、issueを自動でcloseなどしてくれます。  
キーワードに関しては、下記URLを参考にしてください。  
https://docs.github.com/ja/free-pro-team@latest/github/managing-your-work-on-github/linking-a-pull-request-to-an-issue

※issue登録するときは、ラベルを使っていただくと、把握しやすくてありがたいです。


## ■Git操作その他

### mainの差分を吸収したいとき
```
$ git rebase main
```

### git pullでコンフリクトして戻したいとき
```
$ git merge --abort
```


## ■その他
テストコードで標準出力させたい場合  
```
$ cargo test -- --nocapture
```



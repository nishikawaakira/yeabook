# 開発はじめ

新規開発者のためのページ。  
YamatoEventAnalyzerのコードはGithubで管理しています。  
まずは公開されているコードを自らの環境でコンパイル実行することから始めてみましょう。

https://github.com/YamatoSecurity/YamatoEventAnalyzer

- [開発はじめ](#開発はじめ)
  - [環境構築](#環境構築)
  - [コンパイルまでの流れ](#コンパイルまでの流れ)
    - [コードの取得](#コードの取得)
    - [コードの修正](#コードの修正)
    - [コンパイル](#コンパイル)
    - [実行](#実行)
  - [タスクの管理](#タスクの管理)

## 環境構築

各自以下の環境を用意しましょう。

- Rustをコンパイルできる
- Gitコマンドの実行ができる

## コンパイルまでの流れ

### コードの取得

GitHubにて公開されているコードを取得する。

```bash
$ git clone git@github.com:YamatoSecurity/YamatoEventAnalyzer.git
$ cd YamatoEventAnalyzer
```

### コードの修正

コードを修正したい場合には [GitHub運用ルール](github.md) に従ってコードの修正を行う。
とりあえずもとに戻したい場合には `git reset --hard origin/master` を打てばもとに戻る。

### コンパイル

- 実行ファイルの作成
```bash
$ cargo build
```
- テストケースの実行
```bash
$ cargo test
```

### 実行

`cargo build` によって作成された実行ファイルは `/target/debug/yamato_event_analyzer` として作成されます。

## タスクの管理

[GitHub上のissue](https://github.com/YamatoSecurity/YamatoEventAnalyzer/issues)でYEAの開発に関わるタスクを管理しています。  
様々な機能追加や修正箇所がある場合には各自自由に追加して行きましょう。

処理ができるissueがあれば積極的に解決してもらえるとうれしいです。
詳しい書き方等については [issueへの対応](github.md#■issueへの対応) を参照してください。

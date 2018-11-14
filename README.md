# Batch Job Controller

複数のプロセス（Job）を逐次実行するUiPathのワークフローテンプレート。エラーが発生した時のリトライ、スクリーンショット取得、リカバリポイント管理などの機能を持ち、運用しやすいワークフローを実装することができます。ReFrameworkをベースとしているので、制御の内容を簡単に把握することができます。

## 内容

定義ファイル（BatchJobDefinition.xlsx）に記載したプロセスを順次実行します。System Errorで指定回数以上リトライする場合と、Business Errorなら停止し、実行結果ファイル（BatchJobStatus-yyyy-MM-dd.xlsx）に記録します。停止した場合は、ワークフローを再実行するだけで、失敗しているプロセスから処理を再開します。

Data\BatchJobDifinition.xlsx

| #   | Process File  | Process Name | Execution DateTime | Execution Second | Status |
| --- | ------------- | ------------ | ------------------ | ---------------- | ------ |
| 1   | Process1.xaml | プロセス１   | -                  | -                | -      |
| 2   | Process2.xaml | プロセス２   | -                  | -                | -      |
| 3   | Process3.xaml | プロセス３   | -                  | -                | -      |

Data\BatchJobStatus-2018-11-8.xlsx

| #   | Process File  | Process Name | Execution DateTime | Execution Second | Status         |
| --- | ------------- | ------------ | ------------------ | ---------------- | -------------- |
| 1   | Process1.xaml | プロセス１   | 2018/11/8 12:00    | 10               | Success        |
| 2   | Process2.xaml | プロセス２   | 2018/11/8 12:05    | 290              | Business Error |
| 3   | Process3.xaml | プロセス３   | -                  | -                | -              |

* #: プロセス番号（ログに出力します）
* Process File: プロセスファイル名（Processesフォルダの対応するファイル名のワークフローを実行します）
* Process Name: プロセス名称（プロセスの概要。ログに出力します）
* Execution DateTime: プロセスが終了した日時
* Execution Second: プロセスの実行時間（秒数）
* Status: プロセスの実行結果（Success/Business Error/System Error)

## 設定

Data¥Condig.xlsxで以下の設定ができます。

Settingタブ

| Name                     | Default                  | Description                              |
| ------------------------ | ------------------------ | ---------------------------------------- |
| logF_BusinessProcessName | Batch Job Controller | ワークフローの名称（ログに出力されます） |
| Batch_Job_Definition | Data\BatchJobDefinition.xlsx | バッチジョブ定義ファイル名       |
| Batch_Job_Status | Data\BatchJobStatus-{0}.xlsx | バッチジョブ実行結果ファイル名         |

Constantsタブ

| Name           | Default | Description  |
| -------------- | ------- | ------------ |
| MaxRetryNumber | 0       | リトライ回数 |

## 利用方法

1. テンプレート一式をダウンロードします。

2. project.jsonの"name","description"を修正します。

3. Data¥Config.xslxの"logF_BusinessProcessName"を修正します。

4. Data\BatchJobDefinition.xlsxに実行するプロセスのファイル名を記載します。

5. InitProcess.xamlに初期化処理を実装します。

6. EndProces.xamlに終了処理を実装します。

7. Processesフォルダに4.で定義したプロセスを実装します。

※ Processesフォルダには、引数等を設定したテンプレート（ProcessTemplate.xaml）と、デバッグ用にランダムにエラーを発生するプロセス（RandomError.xaml）があらかじめ用意されています。

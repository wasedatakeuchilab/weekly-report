# 週報の半自動化 <!-- omit in toc -->

毎週の週報タスクを効率化します。

- [使い方](#使い方)
  - [リポジトリをクローン](#リポジトリをクローン)
  - [設定ファイルの作成・編集](#設定ファイルの作成編集)
  - [週報の作成](#週報の作成)
  - [週報メールを送信](#週報メールを送信)
- [発展例](#発展例)
  - [週報内容を標準入力から受け取る](#週報内容を標準入力から受け取る)
- [License](#license)

## 使い方

事前に以下のソフトウェアをインストールしておいて下さい。

- [Git](https://git-scm.com/downloads)
- [Docker](https://docs.docker.com/engine/install/)

### リポジトリをクローン

次のコマンドを実行してリポジトリをクローンします。

```console
$ git clone https://github.com/wasedatakeuchilab/weekly-report
$ cd weekly-report
```

### 設定ファイルの作成・編集

次に`default.env`をコピーして、設定ファイル（`.env`）を作成します。

```console
$ cp default.env .env
```

設定ファイルを開き、必須変数である`TO`を埋めてください。
必要があれば、`CC`, `BCC`等のオプショナル変数も設定してください。

```text
TO=foo@example.com
CC=hoge@example.com,fuga@example.com
```

### 週報の作成

先ほど設定した`REPORTDIR`(デフォルト: `./reports`)に`<年>/<月>/<週>.md`のパスでファイルを作成し、
週報の内容をマークダウン形式で書き込んでください。
例えば、2023 年 1 月の第 2 週の週報であればファイルパスは`2023/1/2.md`となります。

```console
$ mkdir -p reports/2023/1
$ cat - << EOF > reports/2023/1/2.md
以下は週報となります。

- 実験系のメンテナンス
- 〇〇についての論文調査

週報は以上です。
EOF
```

> Note: 署名はアプリが付けてくれるのでファイルに書き込む必要はありません。

> Note: プレーンテキスト(`.txt`)や HTML(`.html`)で週報を書きたい場合は、
> 設定ファイルで`FILEPATH`の拡張子を変更してください。

### 週報メールを送信

最後に、次の`docker`コマンドを実行して、第`ceil(実行日/7)`週の週報を送信します。
例えば、1 月 3 日に実行した場合は第 1 週の週報を送信し、1 月 14 日に実行した場合は第 2 週の週報を送信します。

```console
$ docker compose run --rm --service-ports report
```

> Note: 初回実行時は GmailAPI の権限を、ブラウザを通してアプリに与える必要があります。

## 発展例

### 週報内容を標準入力から受け取る

設定ファイルで`FILEPATH=-`とし、`docker`コマンドに`-iT`オプションを追加する。

```console
$ echo "This is a test" | docker compose run -iT --rm --service-ports report
```

## License

[MIT License](./LICENSE)

Copyright (c) 2023 Shuhei Nitta

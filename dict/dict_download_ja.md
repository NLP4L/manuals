# NLP4L-DICT： 辞書のダウンロードとデプロイ



## 概要

NLP4Lの辞書生成統合ツールは、生成された辞書データをファイル出力してダウンロードする機能を提供しています。また、サーバ（検索エンジン）へ直接デプロイする仕組みも提供しています。

ダウンロードとデプロイ機能を有効にするには、Configファイルに以下で説明する設定をするだけです。

生成された辞書データをファイルに出力するには、Writerと呼ばれるコンポーネント（JSONファイル形式とCSVファイル形式へ出力するWriterは標準で提供）を設定します。また、サーバへ直接デプロイするには、Deployerと呼ばれるコンポーネントを設定します。デプロイする必要がない場合、Writerの設定だけでも大丈夫です。WriterとDeployerの設定に関しては、後述します。

![overview_download](images/dict_download_overview.png)

Writerの設定を行うと、GUIツールにDownloadボタンが表示されます。Downloadボタンを押下すると、Writerが実行され、Writerが出力したファイルをブラウザからダウンロードできます。

Deployerの設定を行うと、Deployボタンが表示されます。Deployボタンを押下すると、まず、Writerが実行され、その後、Writerが出力したファイルを、Deployerが処理します。

![scnreenshot_download](images/screenshot_download.png)


## Writer

辞書データをファイル出力するには、Writerの設定を行います。

標準では、JSONファイル形式とCSVファイル形式へ出力するWriterが提供されています。

### JsonFileWriterの設定
以下のコンフィグレーション例を参考にしてください。
```
{
  processors : [
    {
      class : ...
      settings : ...
    }
  ]
  writer : {
    class : org.nlp4l.framework.builtin.JsonFileWriterFactory
    settings : {
      file : "/tmp/nlp4l-ner-dict.json"
    }
  }
}

```
JsonFileWriterで設定可能なsettingsは、以下の通りです。

|name|required|default|description|
|:--|:--:|:--:|:--|
|file|false||Writerの出力先ファイル。<br>省略すると、システムのテンポラリーディレクトリにファイルが自動生成されます。<br>例: "/tmp/result-dict.json"|

### CSVFileWriterの設定
以下のコンフィグレーション例を参考にしてください。
```
{
  processors : [
    {
      class : ...
      settings : ...
    }
  ]
  writer : {
    class : org.nlp4l.framework.builtin.CSVFileWriterFactory
    settings : {
      file : "/tmp/nlp4l-doc-clss-dict.csv"
      separator : ","
      encoding :  "UTF-8"
    }
  }
}

```
CSVFileWriterで設定可能なsettingsは、以下の通りです。

|name|required|default|description|
|:--|:--:|:--:|:--|
|file|false||Writerの出力先ファイル。<br>省略すると、システムのテンポラリーディレクトリにファイルが自動生成されます。<br>例: "/tmp/result-dict.csv"|
|separator|false|","(カンマ)|区切り文字。<br>例："," や "&#124;" など|
|encoding|false|UTF-8|出力ファイルの文字コード<br>例："UTF-8"|

## Deployer

Writerにより出力されたファイルをデプロイするには、Deployerの設定を行います。

標準では、HTTPファイル送信するDeployerが提供されています。

### HttpFileTransferDeployerの設定
以下のコンフィグレーション例を参考にしてください。
```
{
  processors : [
    {
      class : ...
      settings : ...
    }
  ]
  writer : {
    class : org.nlp4l.framework.builtin.CSVFileWriterFactory
    settings : {
      file : "/tmp/nlp4l-doc-clss-dict.csv"
      separator : ","
      encoding :  "UTF-8"
    }
  }
  deployer : {
    class : org.nlp4l.framework.builtin.HttpFileTransferDeployerFactory
    settings : {
      deployToUrl :  "http://localhost:8983/FileReceiverServlet"
      deployToFile : "/opt/solr/nlp4l/doc-class-dict.csv"
    }
  }
}

```
HttpFileTransferDeployerで設定可能なsettingsは、以下の通りです。

|name|required|default|description|
|:--|:--:|:--:|:--|
|deployToUrl|true||デプロイ先のURL。<br>例: "http://localhost:8983/FileReceiverServlet"|
|deployToFile|true||保存先のファイル名。<br>例: "/opt/solr/nlp4l/doc-class-dict.csv"|

HttpFileTransferDeployerを使用する場合、別途、受け手となるサーバ側での設定（Servletなど）が必要です。

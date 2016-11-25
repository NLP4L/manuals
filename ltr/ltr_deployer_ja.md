# NLP4L-LTR： HTTPファイル送信デプロイヤ
## 概要

ランキング学習支援ツールでは、予め実装されたビルトインのデプロイヤとして、HTTPファイル送信デプロイヤ（HttpFileTransferDeployer）が提供されています。

HTTPファイル送信デプロイヤは、学習モデルとして出力されたモデルデータをHTTPプロトコルを使用して検索サーバ(Solr/ES)側へ送信するデプロイヤです。

![deployer](images/ltr_deployer.png)


## Config
HTTPファイル送信デプロイヤを使用するには、Configに、以下のように設定します。


![screenshot_deployer](images/screenshot_deployer.png)

### Deployer Factory Class設定

Deployer Factory Classには、org.nlp4l.ltr.support.procs.HttpFileTransferDeployerFactoryと指定します。

### settings設定

HTTPファイル送信デプロイヤで設定可能なsettingsは、以下の通りです。

|name|required|default|description|
|:--|:--:|:--:|:--|
|deployToUrl|true|-|デプロイ先のURL。<br>例：http://localhost:8983/solr/nlp4l/receive/file|
|deployToFile|true|-|保存先のファイル名。<br>例：collection1/conf/ltr_model.conf|

## サーバ側の設定

デプロイ先の検索サーバ側の設定に関しては、「[NLP4Lのsolrプロジェクト](https://github.com/NLP4L/solr)」を参照してください。


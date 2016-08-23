# NLP4L-LTR ランキング学習支援ツール

## 概要

ランキング学習支援ツールとは、ランキング学習に関する機能を検索サービス（Solr, Elasticsearch）で提供する上で必要となるいくつかの作業を支援するためのツールを提供するものです。

## 概要アーキテクチャー

![NLP4L-LTRアーキテクチャー](images/ltr-architecture.png)

## 機能

| 機能名 | 機能概要 |
|:--|:--|
| Configuration | LTRモデル生成のための定義 <br>＜評価データ生成＞ <br>・クエリファイル指定 <br>・評価方法選択（Pointwise, Pairwise, Listwise） 当初は、Pointwiseのみ <br>＜Feature extraction＞ <br>・Features項目定義 <br>・Features取得方法 <br> <モデル生成> <br>・モデル生成のためのFactoryClass |
| Supervise | Query for evaluationファイルを順次読み込み、取得したクエリを利用して検索を実行し、その結果を評価するための機能である。評価方法として、Pointwise / Pairwise / Listwise をサポートする。（現時点では Pointwiseのみ）この機能はGUIを使って、複数人による並行作業が可能となっている。|
| Feature extraction | Solr/ElasticsearchのLTR-Featureモジュールと連動し、評価データ（クエリ、検索結果）に関するFeatureの値を取得する。取得したFeatureデータは、モデル生成時に利用される。|
| Model creation | 評価データとFeatureデータを利用して、Configurationで設定したモデル生成Factoryクラスを利用してモデルデータを生成する。|
| Model deploy | Solr/ElasticsearchのLTR-ReRankモジュールと連動し、生成したモデルデータを検索サービスに配置し適用する。|







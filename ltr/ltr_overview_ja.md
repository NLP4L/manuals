# NLP4L-LTR ランキング学習支援ツール

## 概要

ランキング学習支援ツールとは、ランキング学習に関する機能を検索サービス（Solr, Elasticsearch）で提供する上で必要となるいくつかの作業を支援するためのツールを提供するものです。

## 概要アーキテクチャー

![NLP4L-LTRアーキテクチャー](images/ltr-architecture.png)

## コンセプト

ランキング学習をとりこんだ検索サービスを提供するためには、いくつかのフェーズにわけて作業を行う必要があります。各フェーズにおいて人の手で行わなければならない作業を効率化するための機能を提供することを目標としています。

1. 評価データ生成
2. Feature抽出
3. モデル生成
4. モデル配置


ランキング学習に関するモデル理論と実装は様々なものが存在しています。このツールでは、特定のモデル理論や実装には依存せず利用することが可能となっています。モデル実装のクラスは、ツール連携のための少しのインターフェイスを実装するだけで、ツールに取り込んで利用可能となります。
また、

## 機能

| 機能名 | 機能概要 |
|:--|:--|
| Configuration | LTRモデル生成のための定義 <br>＜評価データ生成＞ <br>・クエリファイル指定 <br>・評価方法選択（Pointwise, Pairwise, Listwise） 当初は、Pointwiseのみ <br>＜Feature extraction＞ <br>・Features項目定義 <br>・Features取得方法 <br> <モデル生成> <br>・モデル生成のためのFactoryClass |
| Supervise | Query for evaluationファイルを順次読み込み、取得したクエリを利用して検索を実行し、その結果を評価するための機能である。評価方法として、Pointwise / Pairwise / Listwise をサポートする。（現時点では Pointwiseのみ）この機能はGUIを使って、複数人による並行作業が可能となっている。|
| Feature extraction | Solr/ElasticsearchのLTR-Featureモジュールと連動し、評価データ（クエリ、検索結果）に関するFeatureの値を取得する。取得したFeatureデータは、モデル生成時に利用される。|
| Model creation | 評価データとFeatureデータを利用して、Configurationで設定したモデル生成Factoryクラスを利用してモデルデータを生成する。|
| Model deploy | Solr/ElasticsearchのLTR-ReRankモジュールと連動し、生成したモデルデータを検索サービスに配置し適用する。|

## 今後の予定

このツールは、まだ設計＆開発途中となっています。今後、設計と開発を進めていくうえで、順次仕様とプロダクトを公開していきます。






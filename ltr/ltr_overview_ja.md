# NLP4L-LTR ランキング学習支援ツール

## コンセプト

ランキング学習支援ツールとは、ランキング学習に関する機能を検索サービス（Solr, Elasticsearch）で提供する上で必要となるいくつかの作業を支援するためのツールを提供するものです。

![NLP4L-LTRアーキテクチャー](images/ltr-concept.png)

ランキング学習をとりこんだ検索サービスを提供するためには、いくつかのフェーズにわけて作業を行う必要があります。各フェーズにおいて人の手で行わなければならない作業を効率化するための機能を提供することを目標としています。

1. 評価データ生成
2. Feature抽出
3. モデル生成
4. モデル配置


ランキング学習に関するモデル理論と実装は様々なものが存在しています。このツールでは、特定のモデル理論や実装には依存せず利用することが可能となっています。モデル実装のクラスは、ツール連携のための少しのインターフェイスを実装するだけで、ツールに取り込んで利用可能となります。


## アーキテクチャー

ランキング学習支援ツールのアーキテクチャ図を以下に示します。

![アーキテクチャー](images/ltr-architecture.png)

## 機能概要

以下に、ランキング学習支援ツールで提供されている機能の概要を示します。アーキテクチャー図と合わせて、ご覧ください。

###  Config
Configでは、ランキング学習支援ツールを動作させていく上での各種設定を行います。

- 評価方法選択（Pointwise, Pairwise, Listwise） （現在はPointwiseのみサポート）
- 検索サーバ（Solr/Elasticsearch）のURL指定 （現在はSolrのみサポート）
- Features取得方法、Features項目定義
- ドキュメントのid, titile, bodyフィールド名の指定
- 学習モデル生成のためのFactoryClass
- デプロイのためのFactoryClass

###  Query List
Query Listでは、評価対象のクエリーのリストを管理します。

- 評価対象のクエリーの一覧、ステータス
- クエリーファイルからのアップロード登録

###  Annotation
Annotationでは、検索サーバ(Solr/Elasticsearch)に対して、実際にクエリーを実行し、クエリー結果のドキュメント一覧に対して、ランキング評価(アノテーション付け)を行います。アノテーション付けは、関連度合の高いドキュメントに対して、星マークを付けることで行います。

- 検索サーバへのクエリ問い合わせ、結果ドキュメントの一覧表示
- Annotation付けと、Annotationデータ保存

###  Feature extraction

Solr/ElasticsearchのLTR-Featureモジュールと連動し、評価データ（クエリ、検索結果ドキュメント）に関するFeatureの値を取得します。取得したFeatureデータは、モデル生成時に利用されます。

- 評価データ（クエリ、検索結果ドキュメント）に関するFeatureの値の取得
- Solr/ElasticsearchのLTR-Featureモジュールとの連動
- Featureデータの保存

###  Train and Model creation
AnnotationデータとFeatureデータを利用して、トレーニングを行い、モデルデータを生成します。
トレーニングには、Configで設定した学習モデル生成Factoryクラスが利用されるため、様々なモデル実装を利用することができます

- AnnotationデータとFeatureデータを利用した機械学習とモデルの生成
- 生成したモデルのSolr/Elasticsearchへの配備(デプロイ)

生成・配備されたモデルは、Solr/ElasticsearchのLTR-ReRankモジュールと連動して、検索時のReRankに利用されます。


## NLP4Lのsolrプロジェクト

ランキング学習支援ツールは、Solr/Elasticsearch側のLTRモジュールと連動して動作します。（現時点では、Solrのみサポート）

Solr側の機能や設定に関しては、「[NLP4Lのsolrプロジェクト](https://github.com/NLP4L/solr)」を参照してください。



## 標準提供のモデル

NLP4Lでは、予め実装されたビルトインの学習モデル生成クラスが提供されています。

|モデル|アプローチ|説明|
|:--|:--|:--|
|PRank|Pointwise|PRank(Perceptron Ranking)アルゴリズムを利用したモデル|
|RankingSVM|疑似Pairwise|SVM(support vector machine)を用いたモデル。<br>Pointwiseデータから疑似的にpairwiseデータに変換して処理を行う。|

これらの標準提供モデル関しては、 [NLP4L-LTR User's Guide](ltr_users_guide_ja.md)を参照してください。

## ユーザ開発のモデル

NLP4Lでは、ビルトインのモデルだけでなく、ユーザがモデルを開発することも想定しています。

詳しくは、[NLP4L-LTR Programmer's Guide](ltr_programmers_guide_ja.md)を参照してください。

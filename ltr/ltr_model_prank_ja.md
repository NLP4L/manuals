# NLP4L-LTR： PRankモデル

## 概要

ランキング学習支援ツールでは、予め実装されたビルトインの学習モデルの1つとして、PRankモデルが提供されています。

PRankモデルは、Pointwiseアプローチの1つとして、PRank(Perceptron Ranking)アルゴリズムを利用して実装されたモデルです。

ランキング学習支援ツールを使用して、作成・抽出したAnnotationデータとFeatureデータを元に、機械学習トレーニングを実行し、モデルデータを生成します。

![prank](images/ltr_prank.png)


## Config
PRankモデルを使用するには、Configに、以下のように設定します。


![screenshot_model_prank](images/screenshot_model_prank.png)

### Trainer Factory Class設定

Trainer Factory Classには、org.nlp4l.ltr.support.procs.PRankTrainerFactoryと指定します。

### settings設定

PRankモデルで設定可能なsettingsは、以下の通りです。

|name|required|default|description|
|:--|:--:|:--:|:--|
|numIterations|false|2000|トレーニングを繰り返す回数。|

## モデルデータ
PRankモデルのデータは、以下のようなJSON形式で出力されます。

各Featureの重み付け(weight)と、ランキング判定用の境界スコア値(bs)が含まれています。


```
{
  "model" : {
    "name" : "prank",
    "type" : "prank",
    "weights" : [ {
      "name" : "TF in title",
      "weight" : 75
    }, {
      "name" : "TF in body",
      "weight" : -115
    }, {
      "name" : "IDF in title",
      "weight" : -331.22149658203125
    }, {
      "name" : "IDF in body",
      "weight" : -293.4493103027344
    } ],
    "bs" : [ -1230, -694, 44, 44, 1872 ]
  }
}
```

# NLP4L-LTR： Programmer's Guide


## 概要

NLP4Lのランキング学習支援ツールは、ビルトインの学習モデルを利用するだけでなく、ユーザが独自の学習モデルやデプロイヤをプログラム開発することも可能です。

ここでは、開発者向けの情報を提供していきます。


## 開発用ライブラリ

NLP4Lでは、ランキング学習ツールとしてのライブラリ一式とは別に、開発者用に必要なモジュールを切り出したライブラリを提供しています。

以下の設定例を参考に取得してください。
（以下の例は、ライブラリのバージョンをnlp4l-framework-library_2.11-0.5.0とした場合）

SBTの設定例
```
libraryDependencies += "org.nlp4l" % "nlp4l-framework-library_2.11" % "0.5.0"
```
Ivyの設定例
```
<dependency org="org.nlp4l" name="nlp4l-framework-library_2.11" rev="0.5.0"/>
```
Mavenの設定例
```
<dependency>
    <groupId>org.nlp4l</groupId>
    <artifactId>nlp4l-framework-library_2.11</artifactId>
    <version>0.5.0</version>
</dependency>
```


## API Docs

APIドキュメントが以下にありますので、ご参照ください。

* API Docs：http://nlp4l.github.io/



## ユーザ開発の学習モデル

ここでは、ユーザが独自の学習モデル（ここではTrainerと呼ぶ）を実装する方法について説明します。

ランキング学習ツールでは、教師ありデータをトレーニングして、学習モデルを生成します。主な処理がトレーニングであるため、ここではTrainerと呼んでおり、Trainerクラスを実装することになります。

### TrainerFactoryの実装

Trainerを実装するには、まず、Trainerを生成するFactoryクラスであるTrainerFactoryが必要です。

- TrainerFactoryを継承したクラスを作成して、getInstance関数を実装。
- Configurationで設定したsettings(Configクラス)が引数として渡されるため、必要に応じてsettingsよりパラメータを取得。
- Trainerクラスをインスタンス化。

```
import com.typesafe.config.Config
import org.nlp4l.ltr.support.procs._

class MyModelTrainerFactory(settings: Config) extends TrainerFactory(settings) {

  override def getInstance: Trainer = {
    new MyModelTrainer(getIntParam("numIterations", 2000))
  }
}
```

### Trainerの種類

ランキング学習ツールでは、以下の種類のTrainerがあり、Trainerの種類により、実装方法が若干異なります。

|type|description|
|:--|:--|
|Pointwise|Pointwise型のトレーナー。|
|疑似的Pairwise|Pointwise型のデータを疑似的にPairwise型に変換して処理を行うトレーナー。|
|Pairwise|Pairwise型のトレーナー。(現在は未サポート)|

![trainer](images/ltr_trainer.png)

### Trainer(Pointwise)の実装

- PointwiseTrainerを継承したクラスを作成して、train関数を実装。
- train関数の引数として渡される学習用データ(Pointwise型)を受け取り、処理を実装。
- モデルデータを生成して、trainメソッドの戻り値として返す。

##### 引数の説明
|引数|型|description|
|:--|:--|:--|
|featureNames|Array[String]|選択されたFeature名の配列。|
|features|Array[Vector[Float]]|学習データ(アノテーション付けされた各ドキュメント)のFeature値Vectorの配列。<br>Feature値Vectorの順序はfeatureNamesの順。|
|labels|Array[Int]|学習データのラベル値(アノテーション付けされた星の数)の配列。<br>配列の順はfeaturesと同じ順。|
|maxLabel|Int|学習データの最大ラベル値(Configで設定したRelevance Degreeの取り得る最大値)。|
|progress|TrainingProgress|ProgressBarへの通知用インタフェース。|


##### 参考コード
```

class MyModelTrainer(val numIterations: Int) extends PointwiseTrainer  {

  def train(featureNames: Array[String],
            features: Array[Vector[Float]],
            labels: Array[Int],
            maxLabel: Int,
            progress: TrainingProgress) : String = {
     :
    progress.report(10)
     // train and create model
    val modeldata = ...
     :
    progress.report(100)
    modeldata
  }
}
```

### Trainer(疑似的Pairwise)の実装

- PseudoPairwiseTrainerを継承したクラスを作成して、train関数を実装。
- train関数の引数として渡される学習用データ(疑似的Pairwise型)を受け取り、処理を実装。
- モデルデータを生成して、trainメソッドの戻り値として返す。

##### 引数の説明
|引数|型|description|
|:--|:--|:--|
|featureNames|Array[String]|選択されたFeature名の配列。|
|features|Vector[Vector[(Int, Vector[Float])]]|Query毎の学習データ用のFeature値Vectorとラベル値。<br>Query毎に疑似的にPairwiseデータへ変換することを想定して、このようなデータ構造となっている。<br>Query毎のVector[<br>&nbsp;&nbsp;&nbsp;&nbsp;評価データVector[<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(ラベル値, Feature値Vector)<br>&nbsp;&nbsp;&nbsp;&nbsp;]<br>]|
|progress|TrainingProgress|ProgressBarへの通知用インタフェース。|


##### 参考コード


```

class MyModelTrainer(val numIterations: Int) extends PseudoPairwiseTrainer  {

  def train(featureNames: Array[String],
            features: Vector[Vector[(Int, Vector[Float])]],
            progress: TrainingProgress) : String = {
     :
    progress.report(10)
     // train and create model
    val modeldata = ...
     :
    progress.report(100)
    modeldata
  }
}
```


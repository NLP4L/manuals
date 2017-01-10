# NLP4L-LTR: PRank Model 

## Overview

NLP4L-LTR provides a PRank learning-to-rank program, which has been implemented using Perceptron Ranking algorithm, as one of Pointwise approaches.

PRank learning-to-rank is performed based on the Feature data and training data, created/extracted using NLP4L-LTR, in order to create model data.

![prank](images/ltr_prank.png)


## Config
Set the Config as follows to use the PRank program.


![screenshot_model_prank](images/screenshot_model_prank.png)

### Trainer Factory Class Setting

Specify org.nlp4l.ltr.support.procs.PRankTrainerFactory as Trainer Factory Class.

### settings

Settings available for the PRank model are as follows.

|name|required|default|description|
|:--|:--:|:--:|:--|
|numIterations|false|2000|number of times training is repeated|

## Model Data 
The PRank model data will be output in the JSON format as follows.

This includes weight of each Feature and boundary score (bs) for ranking.


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
# NLP4L-LTR: GUI Tool Usage Guide

Followings are explanation of how to use the GUI tool and each screen that are provided by the NLP4L learning-to-rank tool.

The GUI tool has the following 5 menus.

|menu|description|
|:--|:--|
|Config|Specify various settings including the type of learning-to-rank model and the URL of search server (Solr/Elasticsearch).<br>You can save more than one setting and Query/Annotation/Feature/Training will be done according to the specified (Loaded) setting.|
|Query|Manage query character string list.<br>A list of queries used by Annotation is displayed. You can upload and add files that have query character strings. You can also delete unnecessary queries. <br>In addition, you can analyze the log of searches and clicks performed by users in an actual application to import it as annotated query data.|
|Annotation|Use the query character string to search the search server (Solr/Elasticsearch) and list the search results. Annotator evaluates the relevancy of document against the query by giving it stars.|
|Feature|Perform feature extraction on the query for annotated documents.<br>Available features and their names must be preset on the server-side as this feature extraction is performed on the search server (Solr/Elasticsearch). |
|Training|Perform learning-to-rank and create a model file. The created model file can be deployed on the server as well.|

Parts that are displayed on the top of each screen are menus. 

![screenshot_menu](images/screenshot_menu.png)


Followings are the details of each menu.

## Config Screen

You can specify various settings including a type of learning model, relevance degree, and a URL of search server (Solr/Elasticsearch) on the Config screen.

![screenshot_config](images/screenshot_config.png)

Specific settings you can set are as follows.

|Setting|Description|
|:--|:--|
|Name|Configuration Name<br>Specify a name to identify a configuration because the system can save more than one configuration.|
|Annotation Type|pointwise / pairwise / listwise. (Currently, only the pointwise is supported.) |
|Relevance Degree|Set relevance degree. From 1 to 5. <br>The maximum number of stars displayed for ranking in the Annotation screen is determined according to the number specified here.|
|Trainer Factory Class|Specify the FactoryClass of learning-to-rank.<br>Ex: org.nlp4l.ltr.support.procs.PRankTrainerFactory|
|- settings|Setting passed to Trainer Factory Class in the above.<br>Ex: { "numIterations": 2000 }|
|Search URL|The URL of search server (Solr/Elasticsearch). (Currently, only Solr is supported.) <br>A request is sent to a search server with ${query} being replaced with query character string.<br>You must add wt=json because the search results are assumed to be in the JSON format.<br>You can specify other settings including highlight.<br>Ex: http://localhost:8983/solr/collection1/select?q=${query}&wt=json|
|Feature Extract URL|The URL of search server (Solr/Elasticsearch) for Feature extraction. (Currently, only Solr is supported.) <br>You must specify Feature extraction settings on the server-side. <br>Ex: http://localhost:8983/solr/collection1/features|
|Feature Extract Config|The name of configuration file used on the search server during Feature extraction.<br>Ex: ltr_features.conf|
|Document Unique Field|The name of unique field in a document. <br>Ex: id|
|Document Title Field|The name of Title field in a document. <br>Ex: title|
|Document Body Field|The name of Body field in a document. <br>Ex: body|
|Deployer Factory Class|Specify FactoryClass to deploy the created model.<br>Ex: org.nlp4l.ltr.support.procs.HttpFileTransferDeployerFactory|
|- settings|The setting passed to Deployer Factory Class.<br>Ex:<br>{ "deployToUrl": "http://localhost:8983/solr/nlp4l/receive/file", <br> "deployToFile": "collection1/conf/ltr_model.conf" }|

#### New

Clicking the New link on the left sidebar on the screen will enable you to add a new Config.

Set each field and click the Save button to add a new Config.

 (Please note that you must click the Load button after Save in order to perform Query/Annotation/Feature/Training using the added new setting.) 

#### Modify and Delete

On the left sidebar on screen, click the link of configuration that you want to modify. Modify the settings and clicking the Save button will change the setting.

Also, clicking the Delete button will delete the setting. (You cannot delete a setting when a Config to be deleted is Loaded. Load another Config once and select and delete the Config to be deleted.)

#### Switching Configs

You can set more than one Config and switch around.

To switch Config, select a link to Config you want to use from the list of Configs on the left sidebar of  screen and click the Load button. The name of Config currently in use is displayed beside the Config on the top menu.

The learning-to-rank (Query/Annotation/Feature/Training) works according to the current (Loaded) Config setting.

![screenshot_config_current](images/screenshot_config_current.png)


## Query Screen 

The Query screen manages the list of query character strings.

This screen displays the list of query character strings that Annotation uses. "Done" is displayed in the Annotation Status field of an annotated query character string.

Clicking the link of query character string sends you to the Annotation screen.

![screenshot_query](images/screenshot_query.png)


#### Uploading Query Character String File

To add a new query character string list, use the file select button to select a text file that has one query character string in one line and click the upload button.

If there is a query list already exists, uploaded addition will be inserted after the existing data.

Following is an example of query character string list file.

```
bank
London
computer
bank AND America
title:London
:
```

#### Deleting and Clearing a Query

To delete a query, select the checkbox for target query and click the Delete button on the upper left corner of table.

To clear Annotation data for an annotated query, select the checkbox for target query and click the Clear button on the upper left corner of table. Also, to clear Annotation data for all query, select the checkbox for target query and click the Clear All button on the upper right corner of screen. 

#### Import 

To import data into the system, click the Import button. Clicking the Import button will send you to the screen for Data Import.

![screenshot_import](images/screenshot_import.png)

Refer to [Click Log Analysis and Import ](ltr_import_ja.md) for the import function.


## Annotation Screen 

In the Annotation screen, you can perform search using a query character string on the search server (Solr/Elasticsearch), manually annotate the search results document list.

#### Annotation and Save
Annotation is done by adding starts to each document in the search result list.

Read each document in the search result list on the Annotation screen, decide the relevancy or each query, and set the stars in relevance degree field column properly. Try to give more starts to documents you want to place higher in the ranking.

When you have completed annotating (giving stars) a query character string, click the Save button and save the Annotation data (Do not forget to click the Save button because giving stars by itself will not save the data.) 

![screenshot_annotation](images/screenshot_annotation.png)

#### Highlighting

You can Highlight the title and body of search result list as well.

To highlight those fields, you need to receive highlight information from the search server (Solr in this case). You can specify the Search URL in Config.

The following is a Search URL example to show highlighting in yellow background. (Refer to the Solr manual for specifying Query parameter for Search URL) 

```
http://localhost:8983/solr/collection1/select?q=${query}&wt=json&hl=true&hl.fl=title,body&hl.simple.pre=<b style='background:yellow'>&hl.simple.post=</b>&hl.snippets=3&hl.fragsize=300
```

The above setting will display the following Annotation screen.

![screenshot_annotation_hi](images/screenshot_annotation_hl.png)

#### Detailed Display (More Button)
Though only a part of titles and bodies are displayed in the search result list, sometimes you might want to see more detailed information.
When you want to see the detail, clicking the more button (or the link of title) will display a pop-up dialog box and enable you to refer to the all of the values in fields.

You can also perform annotation (giving stars) on the pop-up dialog box.

![screenshot_annotation_more](images/screenshot_annotation_more.png)

#### Query Character String Input Field and Search (Search Button )

You sometimes might want to make more changes and evaluate a query character string after you performed a search using a pre-registered query character string. In such cases, you can make modification on the character string in the query character string input field and click the Search button to annotate the search results of modified query character string.

For example, after you pre-registered a query character string "bank" and performed a search to find out that the query character string isn't proper, you can change the query character string to something like "bank AND London" and click the Search button, annotate the search results, and click the Save button.

The new query character string ("bank AND London") will be automatically added to the end of query list (it does not mean that the original query character string would be rewritten).

![screenshot_annotation_search](images/screenshot_annotation_search.png)

On the Query screen, you can see that a new query character string is added to the end of list.

![screenshot_annotation_savenew](images/screenshot_annotation_savenew.png)

Using these query character string input field, Search, and Save button functions, you can specify any query character string to perform search and add query character string while performing annotation without having any query character string at hand.

#### Next Query (Next Button)

Clicking the Next button selects the next unannotated query character string below the current string in the query list and performs search.

"No queries." will displayed if there is no unannotated query character string below in the list.

#### When Sent from Menu Bar
When you are sent from the "Annotation" link in the Menu bar at the top of screen, unannotated first query character string will automatically be selected.

#### Deleting Query Character String (Delete Button)

Clicking the Delete button will not only clear the annotation data but also will clear the data from the query list.
For example, this function can be used in the cases where you search query character strings and get the search results but decide that those strings are not appropriate for learning.

## Feature Screen

In the Features screen, you can perform feature extraction of each query in the annotated documents.

Features to be extracted, and their names, must be registered beforehand on the server because feature extraction is performed on the search server (Solr/Elasticsearch).

We will not discuss the detail of server side here. Refer to the settings of [NLP4L solr project](https://github.com/NLP4L/solr) for the detail of server settings.

#### Feature Extraction 

Clicking the Extract button will start the feature extraction.

![screenshot_feature](images/screenshot_feature.png)

Extraction is complete when Extraction Status becomes "DONE".

![screenshot_feature_100](images/screenshot_feature_100.png)

First Clear and then Extract if you want to extract features again.


## Training Screen

You can use Query Annotation Data to perform the learning-to-rank training to create a model in the Training screen.

#### New Training and Creating Model

Click the New link on the sidebar on the left-hand side of screen to display the New screen first to perform a new training to create a model.

The list of features extracted beforehand is displayed in Features.

Check the checkbox to select the features you want to use and click the Start button will start a training.

Performing a training and creating a model will be handled by the Trainer created by Trainer Factory Class specified in Config.

![screenshot_training_new](images/screenshot_training_new.png)

You can see the created model data when the training is complete and a model is created.

![screenshot_training](images/screenshot_training.png)

#### Deploying Model File

Click the Deploy button to deploy the created model file. The deploy process is handled by Deployer created by Deployer Factory Class specified in Config.

"Success" is displayed when deploy is complete.

![screenshot_training_deploy](images/screenshot_training_deploy.png)

#### Deleting Model

Clicking the Delete button will delete a created model.




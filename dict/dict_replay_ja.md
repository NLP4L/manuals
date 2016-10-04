# NLP4L-DICT： リプレイ機能

## 概要

NLP4Lの辞書生成統合ツールには、標準でリプレイ機能が提供されます。

NLP4L では NLP や機械学習は完璧ではない、という前提に立っています。そのため、辞書生成統合ツールにはGUIが装備されており、自動生成された辞書をGUIを通じて人手で修正することができます。その修正内容をデータベースに記録しておき、次回以降の辞書生成の際に人手で再度同じ修正をしなくてもすむように、記録された修正内容を自動適用するのがリプレイ機能です。

![overview_replay](images/dict_replay_overview.png)

リプレイ機能があることで、同じ修正を何度も行う必要がなくなり、省力化に役立ちます。

### 利用シナリオ

以下の図では、繰り返し実行するJOBに対して、人間が修正を加えていくケースを想定しています。

- 初回実行のJob#1で自動生成されたDictionaryに対し、人間がGUIツールを使用して、データを修正したとします。ここでは、data#2の自動生成結果である"BBB"を"XXX"に修正したと想定しています。GUIツールでは、ここでDictionaryへの修正を行うと同時に、データベースに修正後の内容を記録します。
- 2回目のJob（Job#2）を実行した際、ReplayProcessorにより、以前修正した内容（data#2の修正後の内容である"XXX"）が、自動的に適用され、Dictionaryが生成されます。
- さらにdata#4に対し修正が行われた場合、以前修正した内容に加えて、今回の修正内容が記録されます。リプレイ機能は、既存データの修正だけでなく、GUIツールを使用した辞書データの新規追加(図の例ではnew#5の追加)や削除(図の例ではdata#3の削除)に対しても、適用されます。
- 3回目のJob（Job#3）を実行した際、ReplayProcessorにより、これまでに修正したすべての内容（図の例ではdata#2とdata#4の修正、および、#new5の追加とdata#3の削除）が自動的に適用され、Dictionaryが生成されます。


![overview_replay](images/dict_replay_use_scenario.png)


## Configuration

リプレイ機能を利用するには、JobのConfigurationに、ReplayProcessorを設定する必要があります。


以下のコンフィグレーション例を参考にしてください。
```
{
  processors : [
    {
      class : ...
      settings : ...
    }
    {
      class : org.nlp4l.framework.builtin.ReplayProcessorFactory
      settings : {
      }
    }
  ]
}

```

## GUI Tool

GUIツールを用いて、自動生成されたDictionaryデータを修正(追加・変更・削除)した場合、Replay列(下図参照)に表示されます。
また、Jobを再実行して、リプレイ処理した結果にも、この情報は引き継がれて、同じように表示されます。


![screenshot_replay](images/screenshot_replay.png)

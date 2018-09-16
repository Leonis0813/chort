設計仕様
========

設計仕様では以下を定義する

- :ref:`reg-int-cls`
- :ref:`reg-int-seq`

.. _reg-int-cls:

モジュール構成
--------------

MVCモデルを利用する

*クラス図*

.. uml:: umls/class.uml

- Model

  - Analysis

    - 分析ジョブの情報を管理するクラス

- View

  - AnalysisView

    - 利用者が分析処理を実行，確認するための画面

- Controller

  - AnalysesController

    - 分析処理を管理するコントローラー

.. _reg-int-seq:

シーケンス
----------

- :ref:`reg-int-seq-execute-analysis`
- :ref:`reg-int-seq-show-analyses`

.. _reg-int-seq-execute-analysis:

過去のレースを分析する
^^^^^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-execute-analysis.uml

1. 利用者がパラメーターを入力して実行ボタンを押下する
2. AnalysisViewがAnalysesControllerのexecuteメソッドを実行する
3. AnalysesControllerがAnalysisを生成してジョブ情報を保存する
4. AnalysesControllerが非同期でAnalysisJobのperform_laterを実行した後，利用者に分析が実行されたことを通知する
5. 分析が完了したらAnalysisJobがAnalysisのstate属性をcompletedに更新する
6. AnalysisMailerのfinishedを実行して利用者にメールを送信する

.. _reg-int-seq-show-analyses:

ジョブ情報を確認する
^^^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-show-analyses.uml

1. 利用者が分析画面を開く
2. AnalysisViewがAnalysesControllerのmanageメソッドを実行する
3. AnalysesControllerがAnalysisクラスのallメソッドを実行してジョブ情報を取得する

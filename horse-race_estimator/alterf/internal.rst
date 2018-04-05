設計仕様
========

設計仕様では以下を定義する

- :ref:`alt-int-cls`
- :ref:`alt-int-seq`
- :ref:`alt-int-sch`

.. _alt-int-cls:

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

.. _alt-int-seq:

シーケンス
----------

- :ref:`alt-int-seq-analyze`
- :ref:`alt-int-seq-read-job`

.. _alt-int-seq-analyze:

過去のレースを分析する
^^^^^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-analyze.uml

1. 利用者がパラメーターを入力して実行ボタンを押下する
2. AnalysisViewがAnalysesControllerのlearnメソッドを実行する
3. AnalysesControllerがAnalysisを生成してジョブ情報を保存する
4. AnalysesControllerが非同期でAnalysisJobのperform_laterを実行した後，利用者に分析が実行されたことを通知する
5. 分析が完了したらAnalysisJobがAnalysisのstate属性をcompletedに更新する
6. AnalysisMailerのfinishedを実行して利用者にメールを送信する

.. _alt-int-seq-read-job:

ジョブ情報を確認する
^^^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-read-job.uml

1. 利用者が分析画面を開く
2. AnalysisViewがAnalysesControllerのmanageメソッドを実行する
3. AnalysesControllerがAnalysisクラスのallメソッドを実行してジョブ情報を取得する

スキーマ定義
------------

- :ref:`alt-int-sch-analyses`

.. _alt-int-sch-analyses:

analysesテーブル
^^^^^^^^^^^^^^^^

分析ジョブ情報を登録するanalysesテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"
   :widths: 10, 10, 20, 20, 10

   "id", "INTEGER", "レースのID", "◯", "◯"
   "num_data", "INTEGER", "学習データ数",,
   "num_tree", "INTEGER", "決定木の数",,
   "num_feature", "INTEGER", "特徴量の数",,
   "state", "STRING", "分析の状態",,
   "created_at", "DATETIME", "分析ジョブ情報の作成日時", "", "○"
   "updated_at", "DATETIME", "分析ジョブ情報の更新日時", "", "○"

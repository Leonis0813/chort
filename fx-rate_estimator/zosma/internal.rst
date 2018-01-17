設計仕様
========

設計仕様では以下を定義する

- :ref:`zos-int-cls`
- :ref:`zos-int-seq`
- :ref:`zos-int-sch`

.. _zos-int-cls:

モジュール構成
--------------

*クラス図*

.. uml:: umls/class.uml

- Rate

  - レートを表すクラス

- CandleStick

  - ローソク足を表すクラス

.. _zos-int-seq:

シーケンス
----------

- :ref:`zos-int-seq-collect`
- :ref:`zos-int-seq-aggregate`

.. _zos-int-seq-collect:

過去のレースを分析する
^^^^^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-collect.uml

1. 利用者がパラメーターを入力して実行ボタンを押下する
2. AnalysisViewがAnalysesControllerのlearnメソッドを実行する
3. AnalysesControllerが非同期でAnalysisJobのperform_laterを実行した後，利用者に分析が実行されたことを通知する
4. AnalysisJobが分析を完了させた後，AnalysisMailerのfinishedを実行して利用者にメールを送信する

.. _zos-int-seq-aggregate:

ジョブ情報を確認する
^^^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-aggregate.uml

1. 利用者が分析画面を開く
2. AnalysisViewがAnalysesControllerのmanageメソッドを実行する
3. AnalysesControllerがAnalysisクラスのallメソッドを実行してジョブ情報を取得する

.. _zos-int-sch:

スキーマ定義
------------

- :ref:`zos-int-sch-rates`

.. _zos-int-sch-rates:

ratesテーブル
^^^^^^^^^^^^^

レース情報を登録するratesテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"
   :widths: 10, 10, 20, 20, 10

   "id", "INTEGER", "レースのID", "◯", "◯"
   "direction", "STRING", "左回りか右回りか",,
   "distance", "INTEGER", "コースの距離",,
   "place", "STRING", "場所",,
   "round", "INTEGER", "ラウンド",,
   "track", "STRING", "芝やダートなど，地面の種類",,
   "weather", "STRING", "天候",,

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

レートを収集する
^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-collect.uml

1. レートのファイル数分MysqlClientクラスのexecute_queryメソッドを実行してレートをDBに登録する

   - 外部ツールのレートはリアルタイムで日付・ペアごとにファイル出力されている

.. _zos-int-seq-aggregate:

指標を生成する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-aggregate.uml

1. 1分間隔でペアごとにレートから1日分のローソク足を作成する

.. _zos-int-sch:

スキーマ定義
------------

- :ref:`zos-int-sch-rates`
- :ref:`zos-int-sch-candle_sticks`

.. _zos-int-sch-rates:

ratesテーブル
^^^^^^^^^^^^^

レートを登録するratesテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"
   :widths: 10, 10, 20, 20, 10

   "id", "INTEGER", "レートのID", "◯", "◯"
   "time", "DATETIME", "レートが変化した日時",, "◯"
   "pair", "STRING", "レートのペア",, "◯"
   "bid", "FLOAT", "売値",, "◯"
   "ask", "FLOAT", "買値",, "◯"

.. _zos-int-sch-candle_sticks:

candle_sticksテーブル
^^^^^^^^^^^^^^^^^^^^^

ローソク足を登録するcandle_sticksテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"
   :widths: 10, 10, 20, 20, 10

   "id", "INTEGER", "ローソク足のID", "◯", "◯"
   "from", "STRING", "開始時間",, "◯"
   "to", "INTEGER", "終了時間",, "◯"
   "pair", "STRING", "ペア",, "◯"
   "interval", "INTEGER", "間隔(5分足，1時間足など)",, "◯"
   "open", "STRING", "始値",, "◯"
   "close", "STRING", "終値",, "◯"
   "high", "STRING", "高値",, "◯"
   "low", "STRING", "安値",, "◯"

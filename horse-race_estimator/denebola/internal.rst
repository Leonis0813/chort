設計仕様
========

設計仕様では以下を定義する

- :ref:`den-int-cls`
- :ref:`den-int-seq`
- :ref:`den-int-sch`

.. _den-int-cls:

モジュール構成
--------------

MVCモデルを利用する

*クラス図*

.. uml:: umls/class.uml

- Race

  - レース情報を表すクラス

- Entry

  - エントリー情報を表すクラス

- Result

  - レース結果を表すクラス

.. _den-int-seq:

シーケンス
----------

- :ref:`den-int-seq-collect`
- :ref:`den-int-seq-aggregate`

.. _den-int-seq-collect:

レース情報を収集する
^^^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-collect.uml

1. レース一覧を取得する
2. レース情報を1ページずつウェブサイトから取得する
3. 取得したウェブページをパースする
4. レース情報をDBに登録する
5. レースのエントリーを1つずつDBに登録する
6. 各エントリーの結果を1つずつDBに登録する
7. レースIDとエントリー番号をDBに登録して素性を生成するためのレコードを作成する

.. _den-int-seq-aggregate:

素性を生成する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-aggregate.uml

- 素性ごとにレコードを更新していく

.. _den-int-sch:

スキーマ定義
------------

- :ref:`den-int-sch-races`
- :ref:`den-int-sch-entries`
- :ref:`den-int-sch-results`
- :ref:`den-int-sch-features`

.. _den-int-sch-races:

racesテーブル
^^^^^^^^^^^^^

レース情報を登録するracesテーブルを定義する

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

.. _den-int-sch-entries:

entriesテーブル
^^^^^^^^^^^^^^^

レースのエントリー情報を登録するentriesテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"
   :widths: 10, 10, 20, 20, 10

   "id", "INTEGER", "エントリーのID", "◯", "◯"
   "number", "INTEGER", "エントリーの番号",,
   "age", "INTEGER", "年齢",,
   "weight", "FLOAT", "体重",,
   "burden_weight", "FLOAT", "斤量",,
   "race_id", "INTEGER", "レース情報の外部キー",,

.. _den-int-sch-results:

resultsテーブル
^^^^^^^^^^^^^^^

レース結果情報を登録するresultsテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"
   :widths: 10, 10, 20, 20, 10

   "id", "INTEGER", "レース結果のID", "◯", "◯"
   "order", "INTEGER", "着順",,
   "race_id", "DATETIME", "レース情報の外部キー",,
   "entry_id", "DATETIME", "エントリー情報の外部キー",,

.. _den-int-sch-features:

featuresテーブル
^^^^^^^^^^^^^^^^

素性を登録するfeaturesテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"
   :widths: 10, 10, 20, 20, 10

   "id", "INTEGER", "素性のID", "◯", "◯"
   "direction", "STRING", "左回りか右回りか",,
   "distance", "INTEGER", "コースの距離",,
   "place", "STRING", "場所",,
   "round", "INTEGER", "ラウンド",,
   "track", "STRING", "芝やダートなど，地面の種類",,
   "weather", "STRING", "天候",,
   "number", "INTEGER", "エントリーの番号",,
   "age", "INTEGER", "年齢",,
   "weight", "FLOAT", "体重",,
   "burden_weight", "FLOAT", "斤量",,
   "race_id", "DATETIME", "レース情報の外部キー",,
   "entry_id", "DATETIME", "エントリー情報の外部キー",,

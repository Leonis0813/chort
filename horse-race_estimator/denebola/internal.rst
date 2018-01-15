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

1. HTTPClientのgetメソッドを実行してレース一覧を取得する
2. HTTPClientのgetメソッドを実行してレース情報を取得する
3. Raceオブジェクトを作成する
4. Entryオブジェクトを作成する
5. Resultオブジェクトを作成する
6. RaceオブジェクトをDBに保存する
7. EntryオブジェクトをDBに保存する
8. ResultオブジェクトをDBに保存する

.. _den-int-seq-aggregate:

素性を生成する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-aggregate.uml

1. MysqlClientのselectメソッドを実行してレース情報登録後の状態のIDを取得する
2. MysqlClientのselectメソッドを実行して素性作成済みのレース情報のIDを取得する
3. Featureオブジェクトを作成する
4. FeatureオブジェクトをDBに登録する
5. Raceオブジェクトのfindメソッドを実行してFeatureオブジェクトのIDと一致するレース情報を取得する
6. Featureオブジェクトのupdate!メソッドを実行して素性を更新する
7. Entryオブジェクトのfindメソッドを実行してFeatureオブジェクトのIDと一致するエントリー情報を取得する
8. Featureオブジェクトのupdate!メソッドを実行して素性を更新する
9. Resultオブジェクトのfindメソッドを実行してFeatureオブジェクトのIDと一致するレース結果情報を取得する
10. Featureオブジェクトのupdate!メソッドを実行して素性を更新する

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

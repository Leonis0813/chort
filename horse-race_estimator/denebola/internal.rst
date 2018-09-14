設計仕様
========

設計仕様では以下を定義する

- :ref:`den-int-cls`
- :ref:`den-int-seq`
- :ref:`den-int-sch`

.. _den-int-cls:

モジュール構成
--------------

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

- 収集開始日と終了日を指定可能

  - 日付はyyyy-mm-ddの形式で指定する
  - 指定された期間に基づいてシーケンス1を繰り返す

- 指定がない場合は以下に従う

  - 収集開始日: 実行した日の1週間前
  - 収集終了日: 実行した日

- 実行例

  .. code-block:: none

     bundle exec ruby collect.rb --from=2018-01-01 --to=2018-01-31

.. _den-int-seq-aggregate:

素性を生成する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-aggregate.uml

1. Raceオブジェクトのpluckメソッドを実行してレース情報登録後の状態のIDを取得する
2. Featureオブジェクトのpluckメソッドを実行して素性作成済みのレース情報のIDを取得する

シーケンス1, 2で取得したIDの差分だけ以下を繰り返す

3. Featureオブジェクトを作成する
4. Raceオブジェクトのfindメソッドを実行してFeatureオブジェクトのIDと一致するレース情報を取得する
5. Entryオブジェクトのfindメソッドを実行してFeatureオブジェクトのIDと一致するエントリー情報を取得する
6. Resultオブジェクトのfindメソッドを実行してFeatureオブジェクトのIDと一致するレース結果情報を取得する
7. 取得した全ての情報を設定してFeatureオブジェクトをDBに登録する

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

   "id", "INTEGER", "レースのID", "○", "○"
   "direction", "STRING", "左回りか右回りか",, "○"
   "distance", "INTEGER", "コースの距離",, "○"
   "grade", "STRING", "グレード",,
   "place", "STRING", "場所",, "○"
   "round", "INTEGER", "ラウンド",, "○"
   "start_time", "DATETIME", "レース日時",, "○"
   "track", "STRING", "芝やダートなど，地面の種類",, "○"
   "weather", "STRING", "天候",, "○"
   "created_at", "DATETIME", "レース情報の作成日時", "", "○"
   "updated_at", "DATETIME", "レース情報の更新日時", "", "○"

.. _den-int-sch-entries:

entriesテーブル
^^^^^^^^^^^^^^^

レースのエントリー情報を登録するentriesテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"
   :widths: 10, 10, 20, 20, 10

   "id", "INTEGER", "エントリーのID", "○", "○"
   "age", "INTEGER", "年齢",, "○"
   "burden_weight", "FLOAT", "斤量",, "○"
   "jockey", "STRING", "騎手",,
   "number", "INTEGER", "エントリーの番号",, "○"
   "weight", "FLOAT", "体重",,
   "weight_diff", "FLOAT", "前走との体重の差分",,
   "race_id", "INTEGER", "レース情報の外部キー",,
   "created_at", "DATETIME", "エントリー情報の作成日時", "", "○"
   "updated_at", "DATETIME", "エントリー情報の更新日時", "", "○"

.. _den-int-sch-results:

resultsテーブル
^^^^^^^^^^^^^^^

レース結果情報を登録するresultsテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"
   :widths: 10, 10, 20, 20, 10

   "id", "INTEGER", "レース結果のID", "○", "○"
   "order", "INTEGER", "着順",, "○"
   "race_id", "INTEGER", "レース情報の外部キー",,
   "entry_id", "INTEGER", "エントリー情報の外部キー",,
   "created_at", "DATETIME", "レース結果情報の作成日時", "", "○"
   "updated_at", "DATETIME", "レース結果情報の更新日時", "", "○"

.. _den-int-sch-features:

featuresテーブル
^^^^^^^^^^^^^^^^

素性を登録するfeaturesテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"
   :widths: 10, 10, 20, 20, 10

   "id", "INTEGER", "素性のID", "○", "○"
   "age", "INTEGER", "年齢",, "○"
   "burden_weight", "FLOAT", "斤量",, "○"
   "direction", "STRING", "左回りか右回りか",, "○"
   "distance", "INTEGER", "コースの距離",, "○"
   "grade", "STRING", "グレード",,
   "jockey", "STRING", "騎手",,
   "number", "INTEGER", "エントリーの番号",, "○"
   "place", "STRING", "場所",, "○"
   "round", "INTEGER", "ラウンド",, "○"
   "start_time", "DATETIME", "レース日時",, "○"
   "track", "STRING", "芝やダートなど，地面の種類",, "○"
   "weather", "STRING", "天候",, "○"
   "weight", "FLOAT", "体重",,
   "weight_diff", "FLOAT", "前走との体重の差分",,
   "race_id", "INTEGER", "レース情報の外部キー",,
   "entry_id", "INTEGER", "エントリー情報の外部キー",,
   "created_at", "DATETIME", "素性の作成日時", "", "○"
   "updated_at", "DATETIME", "素性の更新日時", "", "○"

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

- Horse

  - 競走馬情報を表すクラス

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

指定された期間だけ1〜17を繰り返す

指定された日のレースIDファイルが存在すれば1を実行する

1. ファイルからレースIDリストを取得する

そうでなければ2, 3を実行する

2. 外部サイトからレースIDリストを取得する
3. IDリストをファイルに保存する

取得したレースIDごとに4〜17を繰り返す

指定されたレースIDに対応するファイルが存在すれば4を実行する

4. ファイルからレース情報が書かれたHTMLファイルを読み込む

そうでなければ5, 6を実行する

5. 外部サイトからレース情報を取得する
6. レース情報が書かれたHTMLファイルを保存する

7. HTMLをパースする
8. レース情報を抽出する
9. レース情報がDBに存在しなければ登録する

レースのエントリー数分10〜17を繰り返す

10. エントリー情報がDBに存在しなければ登録する
11. レース結果情報がDBに存在しなければ登録する

競走馬情報が存在しなければ12〜17を実行する
競走馬情報が記載されたファイルが存在すれば12を実行する

12. ファイルから競走馬情報が書かれたHTMLファイルを取得する

そうでなければ13, 14を実行する

13. 外部サイトから競走馬情報を取得する
14. 競走馬情報が書かれたHTMLファイルを保存する

15. HTMLをパースする
16. 競走馬情報を抽出する
17. 競走馬情報をDBに登録する

.. _den-int-seq-aggregate:

素性を生成する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-aggregate.uml

1. Raceオブジェクトのpluckメソッドを実行してレース情報登録後の状態のIDを取得する
2. Featureオブジェクトのpluckメソッドを実行して素性作成済みのレース情報のIDを取得する

シーケンス1, 2で取得したIDの差分だけ以下を繰り返す

3. Raceオブジェクトのfindメソッドを実行してFeatureオブジェクトのIDと一致するレース情報を取得する
4. Entryオブジェクトのfindメソッドを実行してFeatureオブジェクトのIDと一致するエントリー情報を取得する
5. Resultオブジェクトのfind_byメソッドを実行してFeatureオブジェクトのIDと一致するレース結果情報を取得する
6. 取得した全ての情報を設定してFeatureオブジェクトをDBに登録する

.. _den-int-sch:

スキーマ定義
------------

- :ref:`den-int-sch-races`
- :ref:`den-int-sch-entries`
- :ref:`den-int-sch-results`
- :ref:`den-int-sch-horses`
- :ref:`den-int-sch-features`

.. _den-int-sch-races:

racesテーブル
^^^^^^^^^^^^^

レース情報を登録するracesテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"
   :widths: 15, 15, 30, 20, 20

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
   :widths: 15, 15, 30, 20, 20

   "id", "INTEGER", "エントリーのID", "○", "○"
   "age", "INTEGER", "年齢",, "○"
   "sex", "STRING", "性別",, "○"
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
   :widths: 15, 15, 30, 20, 20

   "id", "INTEGER", "レース結果のID", "○", "○"
   "order", "INTEGER", "着順",, "○"
   "race_id", "INTEGER", "レース情報の外部キー",,
   "entry_id", "INTEGER", "エントリー情報の外部キー",,
   "created_at", "DATETIME", "レース結果情報の作成日時", "", "○"
   "updated_at", "DATETIME", "レース結果情報の更新日時", "", "○"

.. _den-int-sch-horses:

horsesテーブル
^^^^^^^^^^^^^^

競走馬情報を登録するhorsesテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"
   :widths: 15, 15, 30, 20, 20

   "id", "INTEGER", "競走馬のID", "○", "○"
   "created_at", "DATETIME", "競走馬情報の作成日時", "", "○"
   "updated_at", "DATETIME", "競走馬情報の更新日時", "", "○"

.. _den-int-sch-features:

featuresテーブル
^^^^^^^^^^^^^^^^

素性を登録するfeaturesテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"
   :widths: 15, 15, 30, 20, 20

   "id", "INTEGER", "素性のID", "○", "○"
   "age", "INTEGER", "年齢",, "○"
   "sex", "STRING", "性別",, "○"
   "burden_weight", "FLOAT", "斤量",, "○"
   "direction", "STRING", "左回りか右回りか",, "○"
   "distance", "INTEGER", "コースの距離",, "○"
   "grade", "STRING", "グレード",,
   "jockey", "STRING", "騎手",,
   "number", "INTEGER", "エントリーの番号",, "○"
   "place", "STRING", "場所",, "○"
   "round", "INTEGER", "ラウンド",, "○"
   "month", "INTEGER", "レース月",, "○"
   "track", "STRING", "芝やダートなど，地面の種類",, "○"
   "weather", "STRING", "天候",, "○"
   "weight", "FLOAT", "体重",,
   "weight_diff", "FLOAT", "前走との体重の差分",,
   "weight_per", "FLOAT", "斤量/体重",,
   "race_id", "INTEGER", "レース情報の外部キー",,
   "entry_id", "INTEGER", "エントリー情報の外部キー",,
   "created_at", "DATETIME", "素性の作成日時", "", "○"
   "updated_at", "DATETIME", "素性の更新日時", "", "○"

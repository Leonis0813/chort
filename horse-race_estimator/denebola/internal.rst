設計仕様
========

設計仕様では以下を定義する

- :ref:`den-int-class`
- :ref:`den-int-sequence`
- :ref:`den-int-schema`

.. _den-int-class:

モジュール構成
--------------

*クラス図*

.. uml:: umls/class.uml

- Race

  - :ref:`den-ext-res-races` を表すクラス

- Entry

  - :ref:`den-ext-res-entries` を表すクラス

- Horse

  - :ref:`den-ext-res-horses` を表すクラス

- Feature

  - :ref:`den-ext-res-features` を表すクラス

.. _den-int-sequence:

シーケンス
----------

- :ref:`den-int-seq-collect`
- :ref:`den-int-seq-extract`
- :ref:`den-int-seq-aggregate`

.. _den-int-seq-collect:

HTMLファイルを収集する
^^^^^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-collect.uml

指定された期間だけ1〜3を繰り返す

指定された日のレースIDファイルが存在すれば1を実行する

1. ファイルからレースIDリストを取得する

そうでなければ2, 3を実行する

2. 外部サイトからレースIDリストを取得する
3. IDリストをファイルに保存する

取得したレースIDごとに4〜8を繰り返す

指定されたレースIDに対応するファイルが存在すれば4を実行する

4. ファイルからレース情報が書かれたHTMLファイルを読み込む

そうでなければ5, 6を実行する

5. 外部サイトからレース情報を取得する
6. レース情報が書かれたHTMLファイルを保存する

レースのエントリー数分7, 8を繰り返す

競走馬情報が存在しなければ7, 8を実行する

7. 外部サイトから競走馬情報を取得する
8. 競走馬情報が書かれたHTMLファイルを保存する

.. _den-int-seq-extract:

競馬情報を抽出する
^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-extract.uml

指定された期間だけ1〜11を繰り返す

1. ファイルからレースIDリストを取得する

取得したレースIDごとに2〜11を繰り返す

2. ファイルからレース情報が書かれたHTMLファイルを読み込む
3. HTMLファイルをパースする
4. レース情報を抽出する
5. レース情報をデータベースに登録する

レースのエントリー数分6〜11を繰り返す

6. エントリー情報を抽出する
7. エントリー情報をデータベースに登録する
8. 競走馬情報が書かれたHTMLファイルを読み込む
9. HTMLファイルをパースする
10. 競走馬情報を抽出する
11. 競走馬情報をデータベースに登録する

.. _den-int-seq-aggregate:

素性を生成する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-aggregate.uml

1. Entryオブジェクトのpluckメソッドを実行してレース情報登録後の状態のIDを取得する
2. Featureオブジェクトのpluckメソッドを実行して素性作成済みのレース情報のIDを取得する

シーケンス1, 2で取得したIDの差分だけ以下を繰り返す

3. Raceオブジェクトのfindメソッドを実行してFeatureオブジェクトのIDと一致するレース情報を取得する
4. Entryオブジェクトのfindメソッドを実行してFeatureオブジェクトのIDと一致するエントリー情報を取得する
5. Horseオブジェクトのfindメソッドを実行してFeatureオブジェクトのIDと一致する競走馬情報を取得する
6. 取得した全ての情報を設定してFeatureオブジェクトをDBに登録する

.. _den-int-schema:

スキーマ定義
------------

- :ref:`den-int-sch-races`
- :ref:`den-int-sch-entries`
- :ref:`den-int-sch-horses`
- :ref:`den-int-sch-features`

.. _den-int-sch-races:

racesテーブル
^^^^^^^^^^^^^

レース情報を登録するracesテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"
   :widths: 15, 15, 30, 20, 20

   "id", "INTEGER", "内部ID", "○", "○"
   "race_id", "STRING", "レースのID"",, "○"
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

   "id", "INTEGER", "内部ID", "○", "○"
   "age", "INTEGER", "年齢",, "○"
   "sex", "STRING", "性別",, "○"
   "burden_weight", "FLOAT", "斤量",, "○"
   "jockey", "STRING", "騎手",,
   "number", "INTEGER", "エントリーの番号",, "○"
   "weight", "FLOAT", "体重",,
   "weight_diff", "FLOAT", "前走との体重の差分",,
   "order", "INTEGER", "着順",, "○"
   "race_id", "INTEGER", "レース情報の外部キー",,
   "created_at", "DATETIME", "エントリー情報の作成日時", "", "○"
   "updated_at", "DATETIME", "エントリー情報の更新日時", "", "○"

.. _den-int-sch-horses:

horsesテーブル
^^^^^^^^^^^^^^

競走馬情報を登録するhorsesテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"
   :widths: 15, 15, 30, 20, 20

   "id", "INTEGER", "内部ID", "○", "○"
   "horse_id", "STRING", "競走馬のID", "", "○"
   "last_race_order", "INTEGER", "1走前の順位",,
   "second_last_race_order", "INTEGER", "2走前の順位",,
   "last_race_final_600m_time", "FLOAT", "前走の上り3ハロンタイム",,
   "created_at", "DATETIME", "競走馬情報の作成日時", "", "○"
   "updated_at", "DATETIME", "競走馬情報の更新日時", "", "○"

.. _den-int-sch-features:

featuresテーブル
^^^^^^^^^^^^^^^^

素性を登録するfeaturesテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"
   :widths: 15, 15, 30, 20, 20

   "id", "INTEGER", "内部ID", "○", "○"
   "age", "INTEGER", "年齢",, "○"
   "sex", "STRING", "性別",, "○"
   "burden_weight", "FLOAT", "斤量",, "○"
   "direction", "STRING", "左回りか右回りか",, "○"
   "distance", "INTEGER", "コースの距離",, "○"
   "grade", "STRING", "グレード",,
   "number", "INTEGER", "エントリーの番号",, "○"
   "place", "STRING", "場所",, "○"
   "round", "INTEGER", "ラウンド",, "○"
   "month", "INTEGER", "レース月",, "○"
   "track", "STRING", "芝やダートなど，地面の種類",, "○"
   "weather", "STRING", "天候",, "○"
   "weight", "FLOAT", "体重",,
   "weight_diff", "FLOAT", "前走との体重の差分",,
   "weight_per", "FLOAT", "斤量/体重",,
   "last_race_order", "INTEGER", "1走前の順位",,
   "second_last_race_order", "INTEGER", "2走前の順位",,
   "last_race_final_600m_time", "FLOAT", "前走の上り3ハロンタイム",,
   "race_id", "INTEGER", "レース情報の外部キー",,
   "entry_id", "INTEGER", "エントリー情報の外部キー",,
   "created_at", "DATETIME", "素性の作成日時", "", "○"
   "updated_at", "DATETIME", "素性の更新日時", "", "○"

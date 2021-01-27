設計仕様
========

設計仕様では以下を定義する

- :ref:`zos-int-class`
- :ref:`zos-int-sequence`
- :ref:`zos-int-schema`

.. _zos-int-class:

モジュール構成
--------------

*クラス図*

.. uml:: umls/class.uml

- Rate

  - :ref:`zos-ext-res-rates` を表すクラス

- CandleStick

  - :ref:`zos-ext-res-candle-sticks` を表すクラス

- MovingAverage

  - :ref:`zos-ext-res-moving-averages` を表すクラス

.. _zos-int-sequence:

シーケンス
----------

- :ref:`zos-int-seq-import-rates`
- :ref:`zos-int-seq-import-candle-sticks`
- :ref:`zos-int-seq-import-moving-averages`

.. _zos-int-seq-import-rates:

レートを収集する
^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-import-rates.uml

1. 作業用の一時ディレクトリを作成する

指定された収集開始日と収集終了日から計算した収集月ごとに2, 3を繰り返す

圧縮ファイルが存在する場合は2を，存在しない場合は3を実行する

2. バックアップディレクトリにある圧縮ファイルを解凍してCSVファイルを取得する
3. 外部ツールが出力したCSVファイルとバックアップディレクトリにあるCSVファイルを取得する

指定された収集開始日と収集終了日から計算した収集日ごとに4〜6を繰り返す

4. 取得したCSVファイルから対象日のファイルを取得する

取得したファイル数分5, 6を繰り返す

5. ファイルに記載されているレート情報を成形して一時ファイルに書き込む
6. 書き込んだ一時ファイルをDBにインポートする

7. 一時ディレクトリを削除する

.. _zos-int-seq-import-candle-sticks:

ローソク足を収集する
^^^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-import-candle-sticks.uml

1. 作業用の一時ディレクトリを作成する

指定された収集開始日と収集終了日から計算した収集月ごとに2, 3を繰り返す

圧縮ファイルが存在する場合は2を，存在しない場合は3を実行する

2. バックアップディレクトリにある圧縮ファイルを解凍してCSVファイルを取得する
3. 外部ツールが出力したCSVファイルとバックアップディレクトリにあるCSVファイルを取得する

指定された収集開始日と収集終了日から計算した収集日ごとに4〜6を繰り返す

4. 取得したCSVファイルから対象日のファイルを取得する

取得したファイル数分5, 6を繰り返す

5. ファイルに記載されているローソク足情報を成形して一時ファイルに書き込む
6. 書き込んだ一時ファイルをDBにインポートする

7. 一時ディレクトリを削除する

.. _zos-int-seq-import-moving-averages:

移動平均を収集する
^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-import-moving-averages.uml

1. 作業用の一時ディレクトリを作成する

指定された収集開始日と収集終了日から計算した収集月ごとに2, 3を繰り返す

圧縮ファイルが存在する場合は2を，存在しない場合は3を実行する

2. バックアップディレクトリにある圧縮ファイルを解凍してCSVファイルを取得する
3. 外部ツールが出力したCSVファイルとバックアップディレクトリにあるCSVファイルを取得する

指定された収集開始日と収集終了日から計算した収集日ごとに4〜6を繰り返す

4. 取得したCSVファイルから対象日のファイルを取得する

取得したファイル数分5, 6を繰り返す

5. ファイルに記載されている移動平均情報を成形して一時ファイルに書き込む
6. 書き込んだ一時ファイルをDBにインポートする

7. 一時ディレクトリを削除する

.. _zos-int-schema:

スキーマ定義
------------

- :ref:`zos-int-sch-rates`
- :ref:`zos-int-sch-candle-sticks`
- :ref:`zos-int-sch-moving-averages`

.. _zos-int-sch-rates:

ratesテーブル
^^^^^^^^^^^^^

レートを登録するratesテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"
   :widths: 10, 10, 20, 20, 10

   "id", "INTEGER", "レートのID", "○", "○"
   "time", "DATETIME", "レートが変化した日時",,"○"
   "pair", "STRING", "レートのペア",,"○"
   "bid", "FLOAT", "売値",,"○"
   "ask", "FLOAT", "買値",,"○"
   "created_at", "DATETIME", "作成日時",,"○"
   "updated_at", "DATETIME", "更新日時",,"○"

.. _zos-int-sch-candle-sticks:

candle_sticksテーブル
^^^^^^^^^^^^^^^^^^^^^

ローソク足を登録するcandle_sticksテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"
   :widths: 10, 10, 20, 20, 10

   "id", "INTEGER", "ローソク足のID", "○", "○"
   "from", "DATETIME", "ローソク足の開始日時",, "○"
   "to", "DATETIME", "ローソク足の終了日時",, "○"
   "pair", "STRING", "レートのペア",, "○"
   "time_frame", "STRING", "時間枠を示すID",, "○"
   "open", "FLOAT", "始値",, "○"
   "close", "FLOAT", "終値",, "○"
   "high", "FLOAT", "高値",, "○"
   "low", "FLOAT", "安値",, "○"
   "created_at", "DATETIME", "作成日時",,"○"
   "updated_at", "DATETIME", "更新日時",,"○"

.. _zos-int-sch-moving-averages:

moving_averagesテーブル
^^^^^^^^^^^^^^^^^^^^^^^

移動平均を登録するmoving_averagesテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"
   :widths: 10, 10, 20, 20, 10

   "id", "INTEGER", "移動平均のID", "○", "○"
   "time", "DATETIME", "移動平均を算出した日時",, "○"
   "pair", "STRING", "通貨ペア",, "○"
   "time_frame", "STRING", "時間枠を示すID",, "○"
   "period", "INTEGER", "移動平均値の算出に使用した期間",, "○"
   "value", "FLOAT", "移動平均値",, "○"
   "created_at", "DATETIME", "作成日時",,"○"
   "updated_at", "DATETIME", "更新日時",,"○"

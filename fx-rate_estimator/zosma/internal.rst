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

.. _zos-int-seq:

シーケンス
----------

- :ref:`zos-int-seq-collect`
- :ref:`zos-int-seq-compress`

.. _zos-int-seq-collect:

レートを収集する
^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-collect.uml

1. 外部ツールからレートが書かれたファイルを取得する

   - 外部ツールのレートはリアルタイムで日付・ペアごとにファイル出力されている
   - 1行に1レートが日時の順番で記載されている

2. ファイルに記載されているレートをDBに登録する処理をファイル数分繰り返す
3. DBから日時を指定してレートを取得する

レートの数が一致していれば以下を実行する

4. DBに登録したレートをファイルに出力する

   - 1日分の全てのペアのレートを日時の順番で1ファイルに出力する
   - 一致していなければエラーを発生させる

5. 外部ツールに保存されているファイルを削除する

.. _zos-int-seq-compress:

レートを圧縮する
^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-compress.uml

1. 圧縮対象のファイルを取得する

   - 1ヶ月分のファイルを取得する

2. 圧縮ファイルを作成する

   - tar.gz 形式のファイルを作成する
   - ファイル名は"YYYY-MM.tar.gz"

3. 圧縮対象のファイルを削除する

.. _zos-int-sch:

スキーマ定義
------------

- :ref:`zos-int-sch-rates`

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

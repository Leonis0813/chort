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

- Payoff

  - :ref:`den-ext-res-payoffs` を表すクラス`

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
6. 払い戻し情報を抽出する
7. 払い戻し情報をデータベースに登録する

レースのエントリー数分8〜13を繰り返す

8. エントリー情報を抽出する
9. エントリー情報をデータベースに登録する
10. 競走馬情報が書かれたHTMLファイルを読み込む
11. HTMLファイルをパースする
12. 競走馬情報を抽出する
13. 競走馬情報をデータベースに登録する

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
- :ref:`den-int-sch-payoffs`
- :ref:`den-int-sch-features`

.. _den-int-sch-races:

racesテーブル
^^^^^^^^^^^^^

レース情報を登録するracesテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   race_id,STRING,レースのID,○
   direction,STRING,左回りか右回りか,○
   distance,INTEGER,コースの距離,○
   grade,STRING,グレード,
   place,STRING,場所,○
   round,INTEGER,ラウンド,○
   start_time,DATETIME,レース日時,○
   track,STRING,芝やダートなど，地面の種類,○
   weather,STRING,天候,○
   created_at,DATETIME,レース情報の作成日時,○
   updated_at,DATETIME,レース情報の更新日時,○

.. _den-int-sch-entries:

entriesテーブル
^^^^^^^^^^^^^^^

レースのエントリー情報を登録するentriesテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   age,INTEGER,年齢,○
   burden_weight,FLOAT,斤量,○
   final_600m_time,FLOAT,上り3ハロンタイム,
   jockey,STRING,騎手,○
   number,INTEGER,エントリーの番号,○
   order,STRING,着順,○
   prize_money,INTEGER,獲得賞金,○
   sex,STRING,性別,○
   weight,FLOAT,体重,
   weight_diff,FLOAT,前走との体重の差分,
   race_id,INTEGER,レースの内部ID,○
   horse_id,INTEGER,競走馬の内部ID,
   created_at,DATETIME,エントリー情報の作成日時,○
   updated_at,DATETIME,エントリー情報の更新日時,○

.. _den-int-sch-horses:

horsesテーブル
^^^^^^^^^^^^^^

競走馬情報を登録するhorsesテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   horse_id,STRING,競走馬のID,○
   running_style,STRING,脚質,○
   created_at,DATETIME,競走馬情報の作成日時,○
   updated_at,DATETIME,競走馬情報の更新日時,○

.. _den-int-sch-payoffs:

payoffsテーブル
^^^^^^^^^^^^^^^

レースの払い戻し情報を登録するpayoffsテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   race_id,INTEGER,レースの内部ID,○
   betting_ticket,STRING,馬券,○
   odds,FLOAT,オッズ,○
   created_at,DATETIME,払い戻し情報の作成日時,○
   updated_at,DATETIME,払い戻し情報の更新日時,○

.. _den-int-sch-features:

featuresテーブル
^^^^^^^^^^^^^^^^

素性を登録するfeaturesテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   age,INTEGER,年齢,○
   average_prize_money,FLOAT,馬の平均賞金獲得額,○
   blank,INTEGER,前回のレースから何日空いたか,○
   burden_weight,FLOAT,斤量,○
   direction,STRING,左回りか右回りか,○
   distance,INTEGER,コースの距離,○
   distance_diff,FLOAT,平均距離との差/平均距離,○
   entry_times,INTEGER,レースの出場回数,○
   grade,STRING,グレード,○
   last_race_order,INTEGER,馬の1走前の順位,○
   month,INTEGER,レース月,○
   number,INTEGER,エントリーの番号,○
   place,STRING,場所,○
   rate_within_third,FLOAT,馬の過去4レースの3着以内に入っていた割合,○
   round,INTEGER,ラウンド,○
   running_style,STRING,馬の脚質,○
   second_last_race_order,INTEGER,馬の2走前の順位,○
   sex,STRING,性別,○
   track,STRING,芝やダートなど，地面の種類,○
   weather,STRING,天候,○
   weight,FLOAT,体重,○
   weight_diff,FLOAT,前走との体重の差分,○
   weight_per,FLOAT,斤量/体重,○
   win_times,INTEGER,馬の勝ち回数,○
   won,TINYINT,1着かどうかを表すラベル,○
   created_at,DATETIME,素性の作成日時,○
   updated_at,DATETIME,素性の更新日時,○

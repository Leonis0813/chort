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

- Collector

  - 収集処理を実行するクラス

- Extractor

  - 抽出処理を実行するクラス

- Aggregator

  - 集約処理を実行するクラス

- ApplicationRecord

  - 全てのクラスの共通処理を集めた抽象クラス

- Race

  - :ref:`den-ext-res-race` を表すクラス

- Entry

  - :ref:`den-ext-res-entry` を表すクラス

- Horse

  - :ref:`den-ext-res-horse` を表すクラス

- Jockey

  - :ref:`den-ext-res-jockey` を表すクラス

- Win

  - :ref:`den-ext-res-win` を表すクラス

- Show

  - :ref:`den-ext-res-show` を表すクラス

- BracketQuinella

  - :ref:`den-ext-res-bracket-quinella` を表すクラス

- Quinella

  - :ref:`den-ext-res-quinella` を表すクラス

- QuinellaPlace

  - :ref:`den-ext-res-quinella-place` を表すクラス

- Exacta

  - :ref:`den-ext-res-exacta` を表すクラス

- Trio

  - :ref:`den-ext-res-trio` を表すクラス

- Trifecta

  - :ref:`den-ext-res-trifecta` を表すクラス

- Feature

  - :ref:`den-ext-res-feature` を表すクラス

- ArgumentUtil

  - 引数で受け取ったパラメーターを取得するクラス

- DenebolaLogger

  - ロガークラス

- NetkeibaClient

  - 競馬情報をサーバーから取得するクラス

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

1. 利用者がスクリプトを実行する
2. 収集処理を実行するクラスのインスタンスを生成する
3. バックアップファイル内の空のファイルを削除する

指定された期間だけ4〜14を繰り返す

4. レースIDリストを取得するために5〜7を実行する

指定された日のレースIDファイルが存在すれば5を実行する

5. ファイルからレースIDリストを取得する

そうでなければ6, 7を実行する

6. 外部サイトからレースIDリストを取得する
7. IDリストをファイルに保存する

取得したレースIDごとに8〜15を繰り返す

8. レース情報を取得するために9〜15を実行する

指定されたレースIDに対応するファイルが存在すれば9を実行する

9. ファイルからレース情報が書かれたHTMLファイルを読み込む

そうでなければ10, 11を実行する

10. 外部サイトからレース情報を取得する
11. レース情報が書かれたHTMLファイルを保存する

12. HTMLファイルをパースする

レースのエントリー数分13〜15を繰り返す

13. 競走馬情報を取得するために14, 15を実行する

対応する競走馬情報が存在しなければ14, 15を実行する

14. 外部サイトから競走馬情報を取得する
15. 競走馬情報が書かれたHTMLファイルを保存する

.. _den-int-seq-extract:

競馬情報を抽出する
^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-extract.uml

1. 利用者がスクリプトを実行する
2. 引数で指定されたoperationパラメーターをチェックする
3. 抽出処理を実行するクラスのインスタンスを生成する

指定された期間だけ4〜27を繰り返す

4. ファイルからレースIDリストを取得するために5を実行する

指定された日付に対応するファイルが存在すれば5を実行する

5. ファイルからレースIDを読み込む

取得したレースIDごとに6〜27を繰り返す

6. レース情報を取得するために7を実行する

指定されたレースIDに対応するファイルが存在すれば7を実行する

7. ファイルからレース情報が書かれたHTMLファイルを読み込む

8. HTMLファイルをパースする
9. レース情報を更新するために9, 10を実行する
10. レース情報を抽出する
11. 抽出したレース情報を登録/更新する

レースのエントリー数分12〜25を繰り返す

12. エントリー情報を更新するために13〜25を実行する
13. エントリー情報を抽出する
14. 抽出したエントリー情報を登録/更新する
15. 対応するレース情報をデータベースから取得して関連付ける
16. 騎手情報を更新するために17を実行する
17. 指定された騎手IDをを登録/更新する

エントリー情報と騎手情報の登録/更新に成功した場合は18, 19を実行する

18. 騎手情報から戦績を取得する
19. 戦績にエントリーを追加する

20. 競走馬情報を更新するために21〜24を実行する

指定された競走馬IDに対応するファイルが存在すれば21〜24を実行する

21. 競走馬情報が書かれたHTMLファイルを読み込む
22. HTMLファイルをパースする
23. 競走馬情報を抽出する
24. 抽出した競走馬情報を登録/更新する

エントリー情報と競走馬情報の登録/更新に成功した場合は25, 26を実行する

25. 競走馬情報から戦績を取得する
26. 戦績にエントリーを追加する

27. 払い戻し情報を抽出する
28. レース情報に払い戻し情報を追加する

.. _den-int-seq-aggregate:

素性を生成する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-aggregate.uml

1. 利用者がスクリプトを実行する
2. 引数で指定されたoperationパラメーターをチェックする
3. 集約処理を実行するクラスのインスタンスを生成する

operationパラメーターがcreateの場合は4〜8を実行する

4. エントリーから素性を生成するメソッドを実行する
5. 対象の期間のエントリー情報を検索する

取得したエントリーだけ6〜8を繰り返す

6. レース，エントリー，競走馬，騎手情報から素性を抽出する
7. それ以外の素性を抽出する

素性の作成に成功した場合は8を実行する

8. 素性をデータベースに登録する

operationパラメーターがupdateの場合は9〜13を実行する

9. 既存の素性を更新するメソッドを実行する
10. 対象のIDの素性情報を検索する

取得した素性だけ11〜13を繰り返す

11. レース，エントリー，競走馬，騎手情報から素性を抽出する
12. それ以外の素性を抽出する

素性の作成に成功した場合は13を実行する

13. 素性を更新する

.. _den-int-schema:

スキーマ定義
------------

- :ref:`den-int-sch-races`
- :ref:`den-int-sch-entries`
- :ref:`den-int-sch-horses`
- :ref:`den-int-sch-jockeys`
- :ref:`den-int-sch-wins`
- :ref:`den-int-sch-shows`
- :ref:`den-int-sch-bracket-quinellas`
- :ref:`den-int-sch-quinellas`
- :ref:`den-int-sch-quinella-places`
- :ref:`den-int-sch-exactas`
- :ref:`den-int-sch-trios`
- :ref:`den-int-sch-trifectas`
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
   race_name,STRING,レースの名前,○
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
   number,INTEGER,エントリーの番号,○
   order,STRING,着順,○
   prize_money,INTEGER,獲得賞金,○
   sex,STRING,性別,○
   weight,FLOAT,体重,
   weight_diff,FLOAT,前走との体重の差分,
   race_id,INTEGER,レースの内部ID,○
   jockey_id,INTEGER,騎手の内部ID,
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

.. _den-int-sch-jockeys:

jockeysテーブル
^^^^^^^^^^^^^^^

騎手情報を登録するjockeysテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   jockey_id,STRING,騎手のID,○
   created_at,DATETIME,騎手情報の作成日時,○
   updated_at,DATETIME,騎手情報の更新日時,○

.. _den-int-sch-wins:

winsテーブル
^^^^^^^^^^^^

レースの単勝情報を登録するwinsテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   race_id,INTEGER,レースの内部ID,○
   odds,FLOAT,オッズ,○
   favorite,INTEGER,人気,○
   number,INTEGER,馬番,○
   created_at,DATETIME,単勝情報の作成日時,○
   updated_at,DATETIME,単勝情報の更新日時,○

.. _den-int-sch-shows:

showsテーブル
^^^^^^^^^^^^^

レースの複勝情報を登録するshowsテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   race_id,INTEGER,レースの内部ID,○
   odds,FLOAT,オッズ,○
   favorite,INTEGER,人気,○
   number,INTEGER,馬番,○
   created_at,DATETIME,複勝情報の作成日時,○
   updated_at,DATETIME,複勝情報の更新日時,○

.. _den-int-sch-bracket-quinellas:

bracket_quinellasテーブル
^^^^^^^^^^^^^^^^^^^^^^^^^

レースの枠連情報を登録するbracket_quinellasテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   race_id,INTEGER,レースの内部ID,○
   odds,FLOAT,オッズ,○
   favorite,INTEGER,人気,○
   bracket_number1,INTEGER,1つ目の枠番,○
   bracket_number2,INTEGER,2つ目の枠番,○
   created_at,DATETIME,枠連情報の作成日時,○
   updated_at,DATETIME,枠連情報の更新日時,○

.. _den-int-sch-quinellas:

quinellasテーブル
^^^^^^^^^^^^^^^^^

レースの馬連情報を登録するquinellasテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   race_id,INTEGER,レースの内部ID,○
   odds,FLOAT,オッズ,○
   favorite,INTEGER,人気,○
   number1,INTEGER,1つ目の馬番,○
   number2,INTEGER,2つ目の馬番,○
   created_at,DATETIME,馬連情報の作成日時,○
   updated_at,DATETIME,馬連情報の更新日時,○

.. _den-int-sch-quinella-places:

quinella_placesテーブル
^^^^^^^^^^^^^^^^^^^^^^^

レースのワイド情報を登録するquinella_placesテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   race_id,INTEGER,レースの内部ID,○
   odds,FLOAT,オッズ,○
   favorite,INTEGER,人気,○
   number1,INTEGER,1つ目の馬番,○
   number2,INTEGER,2つ目の馬番,○
   created_at,DATETIME,ワイド情報の作成日時,○
   updated_at,DATETIME,ワイド情報の更新日時,○

.. _den-int-sch-exactas:

exactasテーブル
^^^^^^^^^^^^^^^

レースの馬単情報を登録するexactasテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   race_id,INTEGER,レースの内部ID,○
   odds,FLOAT,オッズ,○
   favorite,INTEGER,人気,○
   first_place_number,INTEGER,1着の馬番,○
   second_place_number,INTEGER,2着の馬番,○
   created_at,DATETIME,馬単情報の作成日時,○
   updated_at,DATETIME,馬単情報の更新日時,○

.. _den-int-sch-trios:

triosテーブル
^^^^^^^^^^^^^

レースの三連複情報を登録するtriosテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   race_id,INTEGER,レースの内部ID,○
   odds,FLOAT,オッズ,○
   favorite,INTEGER,人気,○
   number1,INTEGER,1つ目の馬番,○
   number2,INTEGER,2つ目の馬番,○
   number3,INTEGER,3つ目の馬番,○
   created_at,DATETIME,三連複情報の作成日時,○
   updated_at,DATETIME,三連複情報の更新日時,○

.. _den-int-sch-trifectas:

trifectasテーブル
^^^^^^^^^^^^^^^^^

レースの三連単情報を登録するtrifectasテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   race_id,INTEGER,レースの内部ID,○
   odds,FLOAT,オッズ,○
   favorite,INTEGER,人気,○
   first_place_number,INTEGER,1着目の馬番,○
   second_place_number,INTEGER,2着目の馬番,○
   third_place_number,INTEGER,3着目の馬番,○
   created_at,DATETIME,三連単情報の作成日時,○
   updated_at,DATETIME,三連単情報の更新日時,○

.. _den-int-sch-features:

featuresテーブル
^^^^^^^^^^^^^^^^

素性を登録するfeaturesテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   age,INTEGER,年齢,○
   blank,INTEGER,前回のレースから何日空いたか,○
   burden_weight,FLOAT,斤量,○
   direction,STRING,左回りか右回りか,○
   distance,INTEGER,コースの距離,○
   distance_diff,FLOAT,平均距離との差/平均距離,○
   entry_times,INTEGER,レースの出場回数,○
   grade,STRING,グレード,○
   horse_average_prize_money,FLOAT,馬の平均賞金獲得額,○
   jockey_average_prize_money,FLOAT,騎手の平均賞金獲得額,○
   jockey_win_rate,FLOAT,騎手の勝率,○
   jockey_win_rate_last_four_races,FLOAT,騎手の過去4走の勝率,○
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

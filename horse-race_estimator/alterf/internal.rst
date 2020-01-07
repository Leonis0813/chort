設計仕様
========

設計仕様では以下を定義する

- :ref:`alt-int-class`
- :ref:`alt-int-sequence`
- :ref:`alt-int-schema`

.. _alt-int-class:

モジュール構成
--------------

MVCモデルを利用する

*クラス図*

.. uml:: umls/class.uml

- Model

  - Analysis

    - 分析ジョブの情報を管理するクラス

  - Prediction

    - 予測ジョブの情報を管理するクラス

  - PredictionResult

    - 予測結果を管理するクラス

  - Evaluation

    - 評価ジョブの情報を管理するクラス

  - Evaluation::Datum

    - 評価データを管理するクラス

- View

  - AnalysisView

    - 利用者が分析処理を実行，確認するための画面

  - PredictionView

    - 利用者が予測処理を実行，確認するための画面

  - EvaluationView

    - 利用者が評価処理を実行，確認するための画面

  - EvaluationResultView

    - 利用者が評価結果を確認するための画面

- Controller

  - AnalysesController

    - 分析処理を管理するコントローラー

  - PredictionsController

    - 予測処理を管理するコントローラー

  - EvaluationsController

    - 評価処理を管理するコントローラー

- Library

  - FeatureExtractor

    - HTMLファイルから素性を抽出するモジュール

  - FeatureUtil

    - レース情報から素性を生成するクラス

  - ModelUtil

    - 指定されたモデルを操作するモジュール

  - NetkaibaClient

    - 外部サイトから情報を取得するためのHTTPクライアント

.. _alt-int-sequence:

シーケンス
----------

- :ref:`alt-int-seq-execute-analysis`
- :ref:`alt-int-seq-show-analyses`
- :ref:`alt-int-seq-execute-prediction`
- :ref:`alt-int-seq-show-predictions`
- :ref:`alt-int-seq-execute-evaluation`
- :ref:`alt-int-seq-show-evaluations`

.. _alt-int-seq-execute-analysis:

レースを分析する
^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-execute-analysis.uml

1. 利用者がパラメーターを入力して実行ボタンを押下する
2. POST /analyses を実行する
3. 必須パラメーターが指定されているかチェックする
4. 分析ジョブ情報を作成する
5. 非同期で分析ジョブを実行する
6. IDから分析ジョブ情報を取得する
7. 分析スクリプトを実行する
8. ファイルから分析結果を読み込む
9. 分析ジョブ情報の素性の数を更新する
10. 分析ジョブIDをファイルに出力する
11. 分析結果をメールで通知する
12. 分析ジョブ情報の状態を官僚にする

.. _alt-int-seq-show-analyses:

分析情報を確認する
^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-show-analyses.uml

1. 利用者が分析画面を開く
2. GET /analyses を実行する
3. 分析ジョブ情報を取得する

.. _alt-int-seq-execute-prediction:

レースを予測する
^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-execute-prediction.uml

1. 利用者がパラメーターを入力して実行ボタンを押下する
2. POST /predictions を実行する
3. 必須パラメーターが指定されているかチェックする
4. ファイルの内容をチェックする
5. 予測ジョブ情報を作成する
6. 予測ジョブ情報をデータベースに保存する
7. モデルとテストデータ（ファイル指定の場合）を出力する
8. 非同期で予測ジョブを実行する
9. IDから予測ジョブ情報を取得する
10. モデルを含む圧縮ファイルを解凍する
11. 12〜14を実行して分析ジョブ情報を予測ジョブ情報に設定する
12. ファイルから分析ジョブIDを読み込む
13. 分析ジョブIDから分析ジョブ情報を取得する
14. 予測ジョブ情報を更新して分析ジョブ情報を紐づける

指定されたテストデータがURLの場合は15〜27を実行する

15. 16〜27を実行して外部サイトの情報から素性を生成する
16. HTTPクライアントを作成する

17. 18〜20を実行してレース情報を取得する
18. URLにアクセスしてHTMLファイルを取得する
19. HTMLファイルからエントリー情報を抽出する
20. HTMLファイルからレース情報を抽出する

レースのエントリーの数だけ21〜27を繰り返す

21. 22〜24を実行して競走馬情報を取得する
22. URLにアクセスしてHTMLファイルを取得する
23. HTMLファイルから競走馬の戦績情報を抽出する
24. HTMLファイルから競走馬情報を抽出する

25. 26, 27を実行して騎手情報を取得する
26. URLにアクセスしてHTMLファイルを取得する
27. HTMLファイルから騎手の戦績情報を抽出する

指定されたテストデータがURL以外の場合は28を実行する

28. ファイルを読み込んで素性を取得する

29. 素性をYAML形式でファイルに出力する
30. レースを予測するスクリプトを実行する
31. 32, 33を実行して予測結果情報を作成する
32. 予測結果が書かれたYAMLファイルを読み込む
33. 予測結果情報をデータベースに保存する

34. 予測ジョブ情報を完了にする

.. _alt-int-seq-show-predictions:

予測情報を確認する
^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-show-predictions.uml

1. 利用者が予測画面を開く
2. GET /predictions を実行する
3. 予測ジョブ情報を取得する

.. _alt-int-seq-execute-evaluation:

モデルを評価する
^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-execute-evaluation.uml

1. 利用者がパラメーターを入力して実行ボタンを押下する
2. POST /evaluations を実行する
3. 必須パラメーターが指定されているかチェックする
4. 指定されたパラメーターか不正かをチェックする
5. 評価ジョブ情報を作成する
6. 評価ジョブ情報をデータベースに保存する
7. 指定されたモデルをファイルに出力する

:ref:`alt-ext-ui-evaluation` でファイル，またはテキストを指定した場合は8を実行する

8. 指定された評価データをファイルに出力する
9. 非同期で評価ジョブを実行する
10. IDから評価ジョブ情報を取得する
11. モデルを含む圧縮ファイルを解凍する
12. 12〜15を実行して分析ジョブ情報を評価ジョブ情報に設定する
13. ファイルから分析ジョブIDを読み込む
14. 分析ジョブIDから分析ジョブ情報を取得する
15. 評価ジョブ情報を更新して分析ジョブ情報を紐づける
16. 17〜22を実行して評価データ情報を作成する

:ref:`alt-ext-ui-evaluation` でランダムを選択した場合は17を実行する

17. データベースからレースIDをランダムに取得する

:ref:`alt-ext-ui-evaluation` で Top20 を選択した場合は18, 19を実行する

18. HTTPクライアントを作成する
19. 外部サイトからレースIDを20件取得する

:ref:`alt-ext-ui-evaluation` で Top20，ランダム以外を選択した場合は20を実行する

20. ファイルからレースIDを取得する

取得したレースIDごとに21〜23を繰り返す

21. レースIDからレース情報を取得する
22. レースIDから正解の素性情報を取得する
23. 評価データ情報をデータベースに保存する

評価データごとに24〜30を繰り返す

24. 25を実行して素性を作成する
25. 評価データ情報から素性を検索する

26. 抽出した素性をYAMLファイルに出力する
27. モデルを予測するスクリプトを実行する

28. 29, 30を実行して予測結果情報を作成する
29. 予測結果が書かれたファイルを読み込む
30. 予測結果情報をデータベースに保存する

31. 評価結果から精度を計算する
32. 評価ジョブ情報の状態を完了にする

.. _alt-int-seq-show-evaluations:

評価情報を確認する
^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-show-evaluations.uml

1. 利用者が評価画面を開く
2. GET /evaluations を実行する
3. 評価ジョブ情報を取得する
4. 利用者が詳細ボタンを押下する
5. GET /evaluations/{evaluation_id} を実行する
6. evaluation_idから評価結果情報を取得する

.. _alt-int-schema:

スキーマ定義
------------

- :ref:`alt-int-sch-analyses`
- :ref:`alt-int-sch-predictions`
- :ref:`alt-int-sch-prediction_results`
- :ref:`alt-int-sch-evaluations`
- :ref:`alt-int-sch-evaluation_data`

.. _alt-int-sch-analyses:

analysesテーブル
^^^^^^^^^^^^^^^^

分析ジョブ情報を登録するanalysesテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   analysis_id,STRING,分析ジョブのID,○
   num_data,INTEGER,学習データ数,○
   num_tree,INTEGER,決定木の数,○
   num_feature,INTEGER,特徴量の数,
   num_entry,INTEGER,エントリーの数,
   state,STRING,分析処理の状態,○
   created_at,DATETIME,分析ジョブ情報の作成日時,○
   updated_at,DATETIME,分析ジョブ情報の更新日時,○

.. _alt-int-sch-predictions:

predictionsテーブル
^^^^^^^^^^^^^^^^^^^

予測ジョブ情報を登録するpredictionsテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   model,STRING,モデルファイル名,○
   test_data,STRING,テストデータのファイル名，またはURL,○
   state,STRING,予測処理の状態,○
   analysis_id,INTEGER,分析ジョブの内部ID,
   created_at,DATETIME,予測ジョブ情報の作成日時,○
   updated_at,DATETIME,予測ジョブ情報の更新日時,○

.. _alt-int-sch-prediction_results:

prediction_resultsテーブル
^^^^^^^^^^^^^^^^^^^^^^^^^^

予測結果情報を登録するprediction_resultsテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   predictable_id,INTEGER,"以下のテーブルの内部ID

   - :ref:`alt-int-sch-predictions`
   - :ref:`alt-int-sch-evaluation_data`",○
   predictable_type,STRING,関連モデル名,○
   number,INTEGER,エントリーの馬番,○
   won,TINYINT,1着かどうか,○
   created_at,DATETIME,予測結果情報の作成日時,○
   updated_at,DATETIME,予測結果情報の更新日時,○

.. _alt-int-sch-evaluations:

evaluationsテーブル
^^^^^^^^^^^^^^^^^^^

評価ジョブ情報を登録するevaluationsテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   evaluation_id,STRING,評価ジョブのID,○
   model,STRING,モデルファイル名,○
   data_source,STRING,評価データの情報源,○
   num_data,INTEGER,評価データ数,○
   state,STRING,評価処理の状態,○
   precision,FLOAT,評価したモデルの適合度,
   recall,FLOAT,評価したモデルの再現率,
   f_measure,FLOAT,評価したモデルのF値,
   analysis_id,INTEGER,分析ジョブの内部ID,
   created_at,DATETIME,評価ジョブ情報の作成日時,○
   updated_at,DATETIME,評価ジョブ情報の更新日時,○

.. _alt-int-sch-evaluation_data:

evaluation_dataテーブル
^^^^^^^^^^^^^^^^^^^^^^^

評価レース情報を登録するevaluation_dataテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   evaluation_id,INTEGER,evaluationsテーブルの内部ID,○
   race_id,STRING,評価したレースのID,○
   race_name,STRING,評価したレースの名前,○
   race_url,STRING,評価したレースのURL,○
   ground_truth,INTEGER,正解,○
   created_at,DATETIME,評価ジョブ情報の作成日時,○
   updated_at,DATETIME,評価ジョブ情報の更新日時,○

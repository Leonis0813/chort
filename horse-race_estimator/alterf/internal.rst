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
3. ファイルの内容をチェックする
4. 予測ジョブ情報を作成する
5. モデルとテストデータ（ファイル指定の場合）を出力する
6. 非同期で予測ジョブを実行する

指定されたテストデータがURLの場合、7〜9を実行する

7. URLにアクセスしてレース情報を取得する

レースのエントリーの数だけ8を繰り返す

8. 外部サイトからエントリー情報を取得する

9. 素性をYAML形式でファイルに出力する

以下のいずれかに当てはまる場合は10〜12を実行する

- エントリー数不定のモデル
- エントリー数固定のモデルで、かつテストデータのエントリー数と等しい場合

10. レースを予測するスクリプトを実行する

レースのエントリーの数だけ11を繰り返す

11. 予測結果情報を作成する

12. 予測ジョブ情報を完了にする

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
3. リクエストパラメーターをチェックする

不正なパラメーターがない場合は4〜7を実行する

4. 評価ジョブ情報を作成する
5. 評価ジョブにモデルを設定して実行中状態にする

:ref:`alt-ext-ui-evaluation` でファイル，またはテキストを指定した場合は6を実行する

6. 指定された評価データをファイルに出力する

7. 非同期で評価ジョブを実行する

8. IDから評価ジョブ情報を取得する
9. 10〜12を実行して評価用データのIDを取得する

:ref:`alt-ext-ui-evaluation` で Top20 を選択した場合は10を実行する

10. 外部サイトからレースIDを20件取得する

:ref:`alt-ext-ui-evaluation` でランダムを選択した場合は11を実行する

11. データベースからレースIDをランダムに取得する

:ref:`alt-ext-ui-evaluation` で Top20，ランダム以外を選択した場合は12を実行する

12. ファイルからレースIDを取得する

取得したレースIDごとに13, 14を繰り返す

13. レースIDからレース情報を検索する
14. 評価データ情報を作成する

評価データごとに15〜21を繰り返す

15. 16を実行して素性を作成する
16. 評価データ情報から素性を検索する

17. 抽出した素性をYAML形式でファイルに出力する
18. モデルを予測するスクリプトを実行する
19. 20, 21を実行して予測結果をファイルから取得する

20. 予測結果が書かれたファイルを読み込む

レースのエントリーの数だけ21を繰り返す

21. 予測結果データを作成する

22. 評価結果から精度を計算する
23. 評価ジョブ情報の状態を完了にする

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
   state,STRING,評価処理の状態,○
   precision,FLOAT,評価したモデルの適合度,
   recall,FLOAT,評価したモデルの再現率,
   f_measure,FLOAT,評価したモデルのF値,
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

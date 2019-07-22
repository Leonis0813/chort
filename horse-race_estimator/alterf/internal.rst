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

  - EvaluationData

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

.. _alt-int-sequence:

シーケンス
----------

- :ref:`alt-int-seq-execute-analysis`
- :ref:`alt-int-seq-show-analyses`
- :ref:`alt-int-seq-execute-prediction`
- :ref:`alt-int-seq-show-predictions`
- :ref:`alt-int-seq-show-pre-result`
- :ref:`alt-int-seq-execute-evaluation`
- :ref:`alt-int-seq-show-evaluations`
- :ref:`alt-int-seq-show-eva-result`

.. _alt-int-seq-execute-analysis:

過去のレースを分析する
^^^^^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-execute-analysis.uml

1. 利用者がパラメーターを入力して実行ボタンを押下する
2. POST /analyses を実行する
3. 分析ジョブ情報を作成する
4. 非同期で分析ジョブを実行する
5. 分析ジョブ情報を実行中にする
6. 分析結果をメールで通知する

.. _alt-int-seq-show-analyses:

分析情報を確認する
^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-show-analyses.uml

1. 利用者が分析画面を開く
2. GET /analyses を実行する
3. 分析ジョブ情報を取得する

.. _alt-int-seq-execute-prediction:

レース結果を予測する
^^^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-execute-prediction.uml

1. 利用者がパラメーターを入力して実行ボタンを押下する
2. POST /predictions を実行する
3. 予測ジョブ情報を作成する
4. 非同期で予測ジョブを実行する

指定されたテストデータがURLの場合、5〜7を実行する

5. URLにアクセスしてレース情報を取得する

レースのエントリーの数だけ6を繰り返す

6. 外部サイトからエントリー情報を取得する

7. 素性をYAML形式でファイルに出力する

レースの1着と予測されたエントリーの数だけ8を繰り返す

8. 予測結果情報を作成する

9. 予測ジョブ情報を完了にする

.. _alt-int-seq-show-predictions:

予測情報を確認する
^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-show-predictions.uml

1. 利用者が予測画面を開く
2. GET /predictions を実行する
3. 予測ジョブ情報を取得する

.. _alt-int-seq-show-pre-result:

予測結果情報を確認する
^^^^^^^^^^^^^^^^^^^^^^

*シーケンス図*

- :ref:`alt-int-seq-show-predictions` と同じ

.. _alt-int-seq-execute-evaluation:

モデルを評価する
^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-execute-evaluation.uml

1. 利用者がパラメーターを入力して実行ボタンを押下する
2. POST /evaluations を実行する
3. 評価ジョブ情報を作成する
4. 評価ジョブにモデルを設定して実行中状態にする
5. 非同期で評価ジョブを実行する
6. IDから評価ジョブ情報を取得する
7. 8, 9を実行して評価用データのIDを取得する

:ref:`alt-ext-ui-evaluation` で Top20 を選択した場合は8を実行する

8. 外部サイトからレース情報を20件取得する

:ref:`alt-ext-ui-evaluation` で Top20 以外を選択した場合は9を実行する

9. ファイルからレース情報を取得する

取得したレースIDごとに10〜16を繰り返す

10. 11を実行して素性を作成する
11. レースIDを使ってデータベースからレース情報を取得する
12. レースIDを使って外部サイトからレース名を取得する
13. レース情報から評価データを作成する
14. 抽出した素性をYAML形式でファイルに出力する
15. 16を実行して評価データに対する予測結果をファイルから取得する
16. 1着と予測された場合は予測結果データを作成する

17. 評価結果から精度を計算する
18. 評価ジョブ情報の状態を完了にする

.. _alt-int-seq-show-evaluations:

評価情報を確認する
^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-show-evaluations.uml

1. 利用者が評価画面を開く
2. GET /evaluations を実行する
3. 評価ジョブ情報を取得する

.. _alt-int-seq-show-eva-result:

評価結果情報を確認する
^^^^^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-show-evaluation-result.uml

1. 利用者は詳細ボタンを押下する
2. GET /evaluations/{evaluation_id} を実行する
3. 評価ジョブ情報と評価結果情報を取得する

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
   num_tree,INTEGER,決定木の数,
   num_feature,INTEGER,特徴量の数,○
   state,STRING,分析処理の状態,○
   created_at,DATETIME,分析ジョブ情報の作成日時,○
   updated_at,DATETIME,分析ジョブ情報の更新日時,,○

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
   number,INTEGER,1着と予測されたエントリーの馬番,○
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
   precision,FLOAT,評価したモデルの精度,
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
   race_name,STRING,評価したレースの名前モデルファイル名,○
   race_url,STRING,評価したレースのURL,○
   ground_truth,INTEGER,正解,○
   created_at,DATETIME,評価ジョブ情報の作成日時,○
   updated_at,DATETIME,評価ジョブ情報の更新日時,○

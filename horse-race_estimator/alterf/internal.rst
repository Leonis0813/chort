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

  - Analysis::Parameter

    - 分析実行時のパラメーターを管理するクラス

  - Analysis::Result

    - 分析結果情報を管理するクラス

  - Analysis::Result::Importance

    - 重要度を管理するクラス

  - Analysis::Result::DecisionTree

    - 決定木を管理するクラス

  - Analysis::Result::DecisionTree::Node

    - 決定木のノードを管理するクラス

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

  - AnalysisResultView

    - 利用者が分析結果を確認するための画面

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

  - Api::AnalysesController

    - 分析情報を管理するコントローラー
    - WebAPI用コントローラー

  - Api::Analyses::ParametersController

    - 分析パラメーター情報を管理するコントローラー
    - WebAPI用コントローラー

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
3. 古い一時ファイルを削除する
4. 必須パラメーターが指定されているかチェックする
5. 分析ジョブ情報を作成する
6. 分析結果情報を作成する
7. 分析パラメーター情報を作成する
8. 分析情報をDBに登録する
9. 非同期で分析ジョブを実行する
10. IDから分析ジョブ情報を取得する
11. 分析ジョブ情報の状態を進行中にする
12. 分析ジョブの状態と実行開始日時をブロードキャストする
13. 分析パラメーター情報をファイルに出力する
14. 分析スクリプトを実行する
15. 分析スクリプトの出力ファイルをチェックする
16. 分析結果情報を出力ファイルからインポートする
17. 重要度情報をファイルから読み込む
18. 重要度情報をDBに登録する
19. 決定木情報をDBに登録する

決定木の数だけ20〜22を繰り返す

20. 決定木の構成をファイルからインポートする
21. ノード情報をファイルから読み込む
22. ノード情報をDBに登録する

23. 分析ジョブ情報の素性の数を更新する
24. 分析ジョブの素性の数をブロードキャストする
25. 分析ジョブIDをファイルに出力する
26. モデルやメタデータなど予測に必要なファイルをまとめてzip化する
27. 素性の情報をまとめてzip化する
28. ダウンロード用ファイルを作成する
29. 分析完了をメールで通知する
30. 分析ジョブ情報の状態を完了にする
31. 分析ジョブの状態をブロードキャストする

.. _alt-int-seq-show-analyses:

分析情報を確認する
^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-show-analyses.uml

1. 利用者が分析画面を開く
2. GET /analyses を実行する
3. 古い一時ファイルを削除する
4. 不正なパラメーターが指定されていないかチェックする
5. パラメーターから検索フォームオブジェクトを作成する
6. 検索フォームオブジェクトからDBを検索するためのクエリを生成する
7. クエリを用いて分析ジョブ情報を検索する
8. 利用者が結果表示ボタンを押下する
9. 分析ジョブ情報を取得する
10. 1ヶ月以上前に作成された一時ファイルを削除する
11. 分析ジョブIDから分析結果情報を取得する

.. _alt-int-seq-execute-prediction:

レースを予測する
^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-execute-prediction.uml

1. 利用者がパラメーターを入力して実行ボタンを押下する
2. POST /predictions を実行する
3. 古い一時ファイルを削除する
4. 必須パラメーターが指定されているかチェックする
5. ファイルの内容をチェックする
6. 予測ジョブ情報を作成する
7. 予測ジョブ情報をDBに登録する
8. モデルとテストデータ（ファイル指定の場合）を出力する
9. 非同期で予測ジョブを実行する
10. IDから予測ジョブ情報を取得する
11. 予測ジョブ情報を進行中にする
12. 予測ジョブの状態と実行開始日時をブロードキャストする
13. モデルを含む圧縮ファイルを解凍する
14. 15〜17を実行して分析ジョブ情報を予測ジョブ情報に設定する
15. ファイルから分析ジョブIDを読み込む
16. 分析ジョブIDから分析ジョブ情報を取得する
17. 予測ジョブ情報を更新して分析ジョブ情報を紐づける

指定されたテストデータがURLの場合は18〜30を実行する

18. 19〜30を実行して外部サイトの情報から素性を生成する
19. HTTPクライアントを作成する

20. 21〜23を実行してレース情報を取得する
21. URLにアクセスしてHTMLファイルを取得する
22. HTMLファイルからエントリー情報を抽出する
23. HTMLファイルからレース情報を抽出する

レースのエントリーの数だけ24〜30を繰り返す

24. 25〜27を実行して競走馬情報を取得する
25. URLにアクセスしてHTMLファイルを取得する
26. HTMLファイルから競走馬の戦績情報を抽出する
27. HTMLファイルから競走馬情報を抽出する

28. 29, 30を実行して騎手情報を取得する
29. URLにアクセスしてHTMLファイルを取得する
30. HTMLファイルから騎手の戦績情報を抽出する

指定されたテストデータがURL以外の場合は31を実行する

31. ファイルを読み込んで素性を取得する

32. 素性をYAML形式でファイルに出力する
33. レースを予測するスクリプトを実行する
34. 35, 36を実行して予測結果情報を作成する
35. 予測結果が書かれたYAMLファイルを読み込む
36. 予測結果情報をデータベースに保存する

37. 予測ジョブ情報を完了にする
38. 予測ジョブの状態と予測結果をブロードキャストする

.. _alt-int-seq-show-predictions:

予測情報を確認する
^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-show-predictions.uml

1. 利用者が予測画面を開く
2. GET /predictions を実行する
3. 古い一時ファイルを削除する
4. 予測ジョブ情報を取得する

.. _alt-int-seq-execute-evaluation:

モデルを評価する
^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-execute-evaluation.uml

1. 利用者がパラメーターを入力して実行ボタンを押下する
2. POST /evaluations を実行する
3. 古い一時ファイルを削除する
4. 必須パラメーターが指定されているかチェックする
5. 指定されたパラメーターか不正かをチェックする
6. 評価ジョブ情報を作成する
7. 評価ジョブ情報をDBに登録する
8. 指定されたモデルをファイルに出力する

:ref:`alt-ext-ui-evaluation` でファイル，またはテキストを指定した場合は9を実行する

9. 指定された評価データをファイルに出力する

10. 非同期で評価ジョブを実行する
11. IDから評価ジョブ情報を取得する
12. 評価ジョブ情報の状態を進行中にする
13. 評価ジョブの状態と実行開始日時をブロードキャストする
14. モデルを含む圧縮ファイルを解凍する
15. 16〜18を実行して分析ジョブ情報を評価ジョブ情報に設定する
16. ファイルから分析ジョブIDを読み込む
17. 分析ジョブIDから分析ジョブ情報を取得する
18. 評価ジョブ情報を更新して分析ジョブ情報を紐づける
19. 20〜26を実行して評価データ情報を作成する

:ref:`alt-ext-ui-evaluation` でランダムを選択した場合は20を実行する

20. データベースからレースIDをランダムに取得する

:ref:`alt-ext-ui-evaluation` で Top20 を選択した場合は21, 22を実行する

21. HTTPクライアントを作成する
22. 外部サイトからレースIDを20件取得する

:ref:`alt-ext-ui-evaluation` で Top20，ランダム以外を選択した場合は23を実行する

23. ファイルからレースIDを取得する

取得したレースIDごとに24〜26を繰り返す

24. レースIDからレース情報を取得する
25. レースIDから正解の素性情報を取得する
26. 評価データ情報をDBに登録する

評価データごとに27〜35を繰り返す

27. 28を実行して素性を作成する
28. 評価データ情報から素性を検索する

29. 抽出した素性をYAMLファイルに出力する
30. モデルを予測するスクリプトを実行する

31. 32, 33を実行して予測結果情報を作成する
32. 予測結果が書かれたファイルを読み込む
33. 予測結果情報をDBに登録する
34. 評価結果から精度を計算する
35. 評価ジョブの精度をブロードキャストする

36. 評価ジョブ情報の状態を完了にする
37. 評価ジョブの状態をブロードキャストする

.. _alt-int-seq-show-evaluations:

評価情報を確認する
^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-show-evaluations.uml

1. 利用者が評価画面を開く
2. 評価ジョブ情報を検索する
3. 古い一時ファイルを削除する
4. 評価ジョブ情報一覧を取得する
5. 利用者が詳細ボタンを押下する
6. 評価ジョブ情報を取得する
7. 1ヶ月以上前に作成された一時ファイルを削除する
8. 評価ジョブIDから評価結果情報を取得する

.. _alt-int-schema:

スキーマ定義
------------

- :ref:`alt-int-sch-analyses`
- :ref:`alt-int-sch-analysis_parameters`
- :ref:`alt-int-sch-analysis_results`
- :ref:`alt-int-sch-analysis_result_importances`
- :ref:`alt-int-sch-analysis_result_decision_trees`
- :ref:`alt-int-sch-analysis_result_decision_tree_nodes`
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
   num_feature,INTEGER,特徴量の数,
   num_entry,INTEGER,エントリーの数,
   state,STRING,分析処理の状態,○
   performed_at,DATETIME,分析ジョブの実行開始日時,
   completed_at,DATETIME,分析ジョブの実行完了日時,
   created_at,DATETIME,分析ジョブ情報の作成日時,○
   updated_at,DATETIME,分析ジョブ情報の更新日時,○

.. _alt-int-sch-analysis_parameters:

analysis_parametersテーブル
^^^^^^^^^^^^^^^^^^^^^^^^^^^

分析時のパラメーターを登録するanalysis_parametersテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   analysis_id,INTEGER,analysesテーブルの内部ID,○
   max_depth,INTEGER,木の深さの最大値,
   max_features,STRING,1つの木に利用する素性の数の最大値,○
   max_leaf_nodes,INTEGER,葉ノードの数の最大値,
   min_samples_leaf,INTEGER,葉ノードに存在するデータの最小値,○
   min_samples_split,INTEGER,中間ノードに存在するデータの最小,○
   num_tree,INTEGER,決定木の数,○
   created_at,DATETIME,分析パラメーター情報の作成日時,○
   updated_at,DATETIME,分析パラメーター情報の更新日時,○

.. _alt-int-sch-analysis_results:

analysis_resultsテーブル
^^^^^^^^^^^^^^^^^^^^^^^^

分析結果情報を登録するanalysis_resultsテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   analysis_id,INTEGER,analysesテーブルの内部ID,○
   created_at,DATETIME,分析結果情報の作成日時,○
   updated_at,DATETIME,分析結果情報の更新日時,○

.. _alt-int-sch-analysis_result_importances:

analysis_result_importancesテーブル
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

重要度を登録するanalysis_result_importancesテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   analysis_result_id,INTEGER,analysis_resultsテーブルの内部ID,○
   feature_name,STRING,素性名,○
   value,FLOAT,重要度の値,○
   created_at,DATETIME,重要度情報の作成日時,○
   updated_at,DATETIME,重要度情報の更新日時,○

.. _alt-int-sch-analysis_result_decision_trees:

analysis_result_decision_treesテーブル
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

決定木を登録するanalysis_result_decision_treesテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   analysis_result_id,INTEGER,analysis_resultsテーブルの内部ID,○
   tree_id,INTEGER,決定木のID,○
   created_at,DATETIME,決定木情報の作成日時,○
   updated_at,DATETIME,決定木情報の更新日時,○

.. _alt-int-sch-analysis_result_decision_tree_nodes:

analysis_result_decision_tree_nodesテーブル
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

決定木のノードを登録するanalysis_result_decision_tree_nodesテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   analysis_result_decision_tree_id,INTEGER,analysis_result_decision_treesテーブルの内部ID,○
   node_id,INTEGER,決定木のノードID,○
   node_type,STRING,ノードの種別,○
   group,STRING,親ノードの閾値に対するグループ,
   feature_name,STRING,素性名,
   threshold,FLOAT,閾値,
   num_win,INTEGER,1着のデータ数,
   num_lose,INTEGER,1着以外のデータ数,
   parent_id,INTEGER,親ノードのノードID,
   created_at,DATETIME,決定木のノード情報の作成日時,○
   updated_at,DATETIME,決定木のノード情報の更新日時,○

.. _alt-int-sch-predictions:

predictionsテーブル
^^^^^^^^^^^^^^^^^^^

予測ジョブ情報を登録するpredictionsテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 15,10,30,15

   id,INTEGER,内部ID,○
   prediction_id,STRING,予測ジョブのID,○
   model,STRING,モデルファイル名,○
   test_data,STRING,テストデータのファイル名，またはURL,○
   state,STRING,予測処理の状態,○
   performed_at,DATETIME,分析ジョブの実行開始日時,
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
   specificity,FLOAT,評価したモデルの特異度,
   f_measure,FLOAT,評価したモデルのF値,
   performed_at,DATETIME,評価ジョブの実行開始日時,
   completed_at,DATETIME,評価ジョブの実行完了日時,
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

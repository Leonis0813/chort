設計仕様
========

設計仕様では以下を定義する

- :ref:`reg-int-cls`
- :ref:`reg-int-seq`
- :ref:`reg-int-sch`

.. _reg-int-cls:

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

- View

  - AnalysisView

    - 利用者が分析処理を実行，確認するための画面

  - PredictionView

    - 利用者が予測処理を実行，確認するための画面

- Controller

  - AnalysesController

    - 分析処理を管理するコントローラー

  - PredictionsController

    - 予測処理を管理するコントローラー

  - Api::PredictionsController

    - 予測処理を管理するコントローラー
    - WebAPI用コントローラー

.. _reg-int-seq:

シーケンス
----------

- :ref:`reg-int-seq-analyze`
- :ref:`reg-int-seq-confirm-analyses`
- :ref:`reg-int-seq-predict`
- :ref:`reg-int-seq-confirm-predictions`

.. _reg-int-seq-analyze:

過去のレートを分析する
^^^^^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-analyze.uml

1. 利用者がパラメーターを入力して実行ボタンを押下する
2. 分析画面がサーバーへ分析ジョブを登録する
3. 必須パラメーターが指定されているか確認する
4. 分析ジョブ情報を作成する
5. 非同期で分析ジョブを実行する
6. IDから分析ジョブを取得する
7. 分析ジョブを実行中状態にする
8. 分析ジョブが実行中状態になったことをブロードキャストする
9. パラメーターをファイルに出力する
10. 分析スクリプトを実行する
11. 分析データの最小値，最大値をファイルから読み込む
12. 分析ジョブ情報に最小値，最大値を登録する
13. 分析ジョブIDをファイルに出力する
14. 分析が完了したことをメールで通知する
15. 分析ジョブを完了状態にする
16. 分析ジョブが完了状態になったことをブロードキャストする

.. _reg-int-seq-confirm-analyses:

分析ジョブ情報を確認する
^^^^^^^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-confirm-analyses.uml

1. 利用者が分析画面を開く
2. AnalysisViewがAnalysesControllerのmanageメソッドを実行して分析ジョブ情報を取得する
3. 登録されているローソク足の期間を取得する
4. 登録されている移動平均線の期間を取得する
5. 分析ジョブ情報を生成する
6. DBに登録されている分析ジョブ情報を取得する

.. _reg-int-seq-predict:

レートを予測する
^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-predict.uml

1. 利用者がモデルを入力して実行ボタンを押下する
2. 予測画面がサーバーへ予測ジョブを登録する
3. 必須パラメーターが指定されているか確認する
4. 予測ジョブ情報を生成する
5. 入力されたモデルを保存する
6. 予測ジョブを非同期で実行する
7. IDから予測ジョブを取得する
8. 予測ジョブを実行中状態にする
9. 予測ジョブが実行中状態になったことをブロードキャストする
10. 保存されたモデルファイルを解凍する
11. 予測ジョブと分析ジョブを紐付けるために12〜15を実行する
12. メタデータから分析IDを読み込む
13. 分析IDから分析情報を検索する
14. 予測ジョブに分析情報を設定して更新する
15. 分析情報のペア情報をブロードキャストする
16. 予測パラメーターをファイルに出力する

最新データを使って自動予測を行う場合は17〜19を行う

17. 最新のデータをポーリングするために18〜19を行う
18. 最新のローソク足情報が登録されたか確認する
19. 最新の移動平均線情報が登録されたか確認する

20. 予測スクリプトを実行する
21. 予測結果を保存するために22〜24を実行する
22. 予測結果が書かれたファイルを読み込む
23. 読み込んだファイルに書かれている情報DBに登録してジョブの状態を更新する
24. 予測結果をブロードキャストする
25. 予測ジョブを完了状態にする
26. 予測ジョブが完了状態になったことをブロードキャストする

.. _reg-int-seq-confirm-predictions:

予測ジョブ情報を確認する
^^^^^^^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-confirm-predictions.uml

1. 利用者が分析画面を開く
2. PredictionViewがPredictionsControllerのmanageメソッドを実行する
3. PredictionsControllerがPredictionクラスのallメソッドを実行してジョブ情報を取得する

.. _reg-int-sch:

スキーマ定義
------------

- :ref:`reg-int-sch-analyses`
- :ref:`reg-int-sch-predictions`

.. _reg-int-sch-analyses:

analysesテーブル
^^^^^^^^^^^^^^^^

分析ジョブ情報を登録するanalysesテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 20,20,20,10

   id,INTEGER,内部ID,○
   analysis_id,STRING,分析ジョブのID,○
   from,DATETIME,分析対象期間の開始日時,○
   to,DATETIME,分析対象期間の終了日時,○
   pair,STRING,分析するレートのペア,○
   batch_size,INTEGER,バッチサイズ,○
   min,FLOAT,分析に使用したデータの最小値,
   max,FLOAT,分析に使用したデータの最大値,
   state,STRING,分析の状態,○
   performed_at,DATETIME,分析ジョブの実行開始日時,
   created_at,DATETIME,分析ジョブ情報の作成日時,○
   updated_at,DATETIME,分析ジョブ情報の更新日時,○

.. _reg-int-sch-predictions:

predictionsテーブル
^^^^^^^^^^^^^^^^^^^

予測ジョブ情報を登録するpredictionsテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL
   :widths: 20,10,20,10

   id,INTEGER,内部ID,○
   prediction_id,STRING,予測ジョブのID,○
   model,STRING,モデルファイル名,○
   from,DATETIME,予測対象の開始日時,
   to,DATETIME,予測対象の終了日時,
   pair,STRING,予測するペア,
   means,STRING,予測の実行方法,○
   result,STRING,予測結果,
   state,STRING,予測処理の状態,○
   performed_at,DATETIME,予測ジョブの実行開始日時,
   analysis_id,INTEGER,分析ジョブの内部ID,
   created_at,DATETIME,予測ジョブ情報の作成日時,○
   updated_at,DATETIME,予測ジョブ情報の更新日時,○

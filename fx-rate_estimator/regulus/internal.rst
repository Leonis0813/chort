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
2. AnalysisViewがAnalysesControllerのexecuteメソッドを実行する
3. AnalysesControllerがAnalysisを生成してジョブ情報を保存する
4. AnalysesControllerが非同期でAnalysisJobのperform_laterを実行した後，利用者に分析が実行されたことを通知する
5. 分析が完了したらAnalysisJobがAnalysisのstate属性をcompletedに更新する
6. AnalysisMailerのfinishedを実行して利用者にメールを送信する

.. _reg-int-seq-confirm-analyses:

分析ジョブ情報を確認する
^^^^^^^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-confirm-analyses.uml

1. 利用者が分析画面を開く
2. AnalysisViewがAnalysesControllerのmanageメソッドを実行して分析ジョブ情報を取得する
3. 分析ジョブ情報を生成する
4. 登録されているローソク足の期間を取得する
5. 登録されている移動平均線の期間を取得する
6. DBに登録されている分析ジョブ情報を取得する

.. _reg-int-seq-predict:

レートを予測する
^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-predict.uml

1. 利用者がモデルを入力して実行ボタンを押下する
2. PredictionViewがPredictionsControllerのexecuteメソッドを実行して予測ジョブを登録する
3. 必須パラメーターが指定されているか確認する
4. 予測ジョブ情報を生成する
5. 入力されたモデルを保存する
6. 予測ジョブを非同期で実行する
7. idから予測ジョブ情報を取得する
8. 保存されたモデルファイルを解凍する
9. メタデータからペアを取得する
10. 予測パラメーターをファイルに出力する
11. ペアを予測ジョブ情報に追加する

最新データを使って自動予測を行う場合は12〜14を行う

12. 最新のデータをポーリングするために13〜14を行う
13. 最新のローソク足情報が登録されたか確認する
14. 最新の移動平均線情報が登録されたか確認する

15. 予測スクリプトを実行する
16. 予測結果をファイルから読み込む
17. 予測結果をDBに登録してジョブの状態を更新する

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
   from,DATETIME,分析対象期間の開始日時,○
   to,DATETIME,分析対象期間の終了日時,○
   pair,STRING,分析するレートのペア,○
   batch_size,INTEGER,バッチサイズ,○
   state,STRING,分析の状態,○
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
   model,STRING,モデルファイル名,○
   from,DATETIME,予測対象の開始日時,
   to,DATETIME,予測対象の終了日時,
   pair,STRING,予測するペア,
   means,STRING,予測の実行方法,○
   result,STRING,予測結果,
   state,STRING,予測処理の状態,○
   created_at,DATETIME,予測ジョブ情報の作成日時,○
   updated_at,DATETIME,予測ジョブ情報の更新日時,○

予測ジョブ情報管理API
=====================

本APIでは以下の機能を提供する

- :ref:`reg-ext-api-pre-index`

.. _reg-ext-api-pre-index:

予測ジョブを検索する
^^^^^^^^^^^^^^^^^^^^

.. http:get:: /predictions

   - リクエストクエリ

     - オプション

       - means

         - 実行方法が一致する予測ジョブを検索する
         - 以下のいずれかを指定可能

           - manual: 手動で実行したジョブを検索する
           - auto: 自動で実行したジョブを検索する

       - page

         - 指定したページの予測ジョブを返却する
         - デフォルト 1
         - 最大ページより大きい数を指定した場合は空配列を返却する

       - pair

         - 指定したペアの予測ジョブを返却する
         - :ref:`reg-ext-res-prediction` のペアを参照

       - per_page

         - 指定した数の予測ジョブを返却する
         - デフォルト 10
         - 以下の場合，返却する数は指定した数よりも少なくなる可能性がある

           - pageパラメーターで最終ページを指定していた場合
           - 指定した数の予測ジョブ情報が登録されていない場合

   - レスポンスボディ

     - predictions

       - :ref:`reg-ext-res-prediction` の配列
       - created_atの降順でソートされている

   - ステータスコード

     - 成功時

       - 200

     - 失敗時

       - 400

   **リクエスト例**

   .. sourcecode:: http

      GET /predictions?means=auto HTTP/1.1

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      {
        "predictions": [
          {
            "prediction_id": "60a830b4b980f68ab0828517f806e680",
            "model": "model.zip",
            "from": "2019-08-01T00:00:00.000Z",
            "to": "2019-08-01T23:59:59.000Z",
            "pair": "USDJPY",
            "means": "auto",
            "result": "up",
            "state": "completed",
            "created_at": "2019-08-02T09:00:00.000Z"
          }
        ]
      }

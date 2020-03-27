予測ジョブ情報管理API
=====================

本APIでは以下の機能を提供する

- :ref:`alt-ext-api-pre-index`

.. _alt-ext-api-pre-index:

予測ジョブを検索する
^^^^^^^^^^^^^^^^^^^^

.. http:get:: /predictions

   - リクエストクエリ

     - オプション

       - page (string)

         - 指定したページの予測ジョブを返却する
         - デフォルト 1
         - 最大ページより大きい数を指定した場合は空配列を返却する

   - レスポンスボディ

     - predictions

       - :ref:`alt-ext-res-prediction` の配列

   - ステータスコード

     - 成功時

       - 200

     - 失敗時

       - 400

   **リクエスト例**

   .. sourcecode:: http

      GET /predictions?page=2 HTTP/1.1

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      {
        "predictions": [
          {
            "prediction_id": "2d44e728b365a0c8f91987c39117cc08",
            "performed_at": "2020/03/23 15:14:10",
            "model": "model.zip",
            "test_data": "test_data.yml",
            "state": "completed",
            "results: [
              {
                "number": 1,
                "won": false
              },
              {
                "number": 2,
                "won": true
              }
            ]
          }
        ]
      }

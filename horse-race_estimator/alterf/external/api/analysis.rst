分析ジョブ情報管理API
=====================

本APIでは以下の機能を提供する

- :ref:`alt-ext-api-ana-index`

.. _alt-ext-api-ana-index:

分析ジョブを検索する
^^^^^^^^^^^^^^^^^^^^

.. http:get:: /analyses

   - リクエストクエリ

     - オプション

       - page (string)

         - 指定したページの分析ジョブを返却する
         - デフォルト 1
         - 最大ページより大きい数を指定した場合は空配列を返却する

   - レスポンスボディ

     - analyses

       - :ref:`alt-ext-res-analysis` の配列

   - ステータスコード

     - 成功時

       - 200

     - 失敗時

       - 400

   **リクエスト例**

   .. sourcecode:: http

      GET /analyses?page=2 HTTP/1.1

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      {
        "analyses": [
          {
            "analysis_id": "2d44e728b365a0c8f91987c39117cc08",
            "performed_at": "2020/03/23 15:14:10",
            "num_data": 100,
            "num_tree": 10,
            "num_feature": 27,
            "num_entry": 9,
            "state": "processing"
          }
        ]
      }

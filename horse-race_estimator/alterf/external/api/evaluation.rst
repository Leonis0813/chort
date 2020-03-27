評価ジョブ情報管理API
=====================

本APIでは以下の機能を提供する

- :ref:`alt-ext-api-eva-read`
- :ref:`alt-ext-api-eva-index`

.. _alt-ext-api-eva-read:

評価ジョブを取得する
^^^^^^^^^^^^^^^^^^^^

.. http:get:: /evaluations/[evaluation_id]

   - パスパラメーター

     - evaluation_id

       - :ref:`alt-ext-res-evaluation` の評価ジョブID参照

   - レスポンスボディ

     - :ref:`alt-ext-res-evaluation`

   - ステータスコード

     - 成功時

       - 200

     - 失敗時

       - 404

   **リクエスト例**

   .. sourcecode:: http

      GET /evaluations/2d44e728b365a0c8f91987c39117cc08 HTTP/1.1

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      {
        "evaluation_id": "2d44e728b365a0c8f91987c39117cc08",
        "performed_at": "2020/03/23 15:14:10",
        "model": "model.zip",
        "data_source": "random",
        "num_data": 100,
        "state": "completed",
        "precision": 0.63,
        "recall": 0.78,
        "f_measure": 0.70,
        "data": [
          {
            "race_id": "201409010811",
            "race_name": "レース名",
            "url": "https://race.example.com/races/201409010811",
            "prediction_results": [
              {
                "number": 1,
                "won": true
              },
              {
                "number": 2,
                "won": false
              }
            ],
            "ground_truth": 1
          }
        ]
      }

.. _alt-ext-api-eva-index:

評価ジョブを検索する
^^^^^^^^^^^^^^^^^^^^

.. http:get:: /evaluations

   - リクエストクエリ

     - オプション

       - page (string)

         - 指定したページの評価ジョブを返却する
         - デフォルト 1
         - 最大ページより大きい数を指定した場合は空配列を返却する

   - レスポンスボディ

     - predictions

       - 結果を除く :ref:`alt-ext-res-prediction` の配列

   - ステータスコード

     - 成功時

       - 200

     - 失敗時

       - 400

   **リクエスト例**

   .. sourcecode:: http

      GET /evaluations?page=2 HTTP/1.1

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      {
        "evaluations": [
          {
            "evaluation_id": "2d44e728b365a0c8f91987c39117cc08",
            "performed_at": "2020/03/23 15:14:10",
            "model": "model.zip",
            "data_source": "random",
            "num_data": 100,
            "state": "processing",
            "precision": null,
            "recall": null,
            "f_measure": null
          }
        ]
      }

分析情報管理API
===============

本APIでは以下の機能を提供する

- :ref:`alt-ext-api-ana-show`

.. _alt-ext-api-ana-show:

分析情報を取得する
^^^^^^^^^^^^^^^^^^

.. http:get:: /analyses/[analysis_id]

   - レスポンスボディ

     - :ref:`alt-ext-res-analysis`

   - ステータスコード

     - 成功時

       - 200

     - 失敗時

       - 404

   **リクエスト例**

   .. sourcecode:: http

      GET /analyses/15f61ba31273e7c342dd0934f894f0a0 HTTP/1.1

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      {
        "analysis_id": "15f61ba31273e7c342dd0934f894f0a0",
        "performed_at": "2020-07-24T11:41:04.000Z",
        "num_data": 100,
        "num_feature": 27,
        "num_entry": null,
        "state": "completed",
        "parameter": {
          "num_tree": 10,
          "max_depth": null,
          "min_samples_split": 2,
          "min_samples_leaf": 1,
          "max_features": "sqrt",
          "max_leaf_nodes": null
        },
        "result": {
          "importances": [
            {
              "feature_name": "win_times",
              "value": 0.233369
            }
          ]
        }
      }

分析情報管理API
===============

本APIでは以下の機能を提供する

- :ref:`alt-ext-api-ana-show`
- :ref:`alt-ext-api-ana-par-show`

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
          "max_depth": null,
          "max_features": "sqrt",
          "max_leaf_nodes": null,
          "min_samples_leaf": 1,
          "min_samples_split": 2,
          "num_tree": 10
        },
        "result": {
          "importances": [
            {
              "feature_name": "win_times",
              "value": 0.233369
            }
          ],
          "decision_trees": [
            {
              "tree_id": 0,
              "nodes": [
                {
                  "node_id": 0,
                  "node_type": "root",
                  "feature_name": "weight_per",
                  "threshold": 0.79,
                  "left_node_id": 1,
                  "right_node_id": 2
                },
                {
                  "node_id": 1,
                  "node_type": "leaf",
                  "feature_name": "horse_average_prize_money",
                  "threshold": 0.59,
                  "left_node_id": null,
                  "right_node_id": null
                },
                {
                  "node_id": 2,
                  "node_type": "leaf",
                  "feature_name": "jockey_win_rate",
                  "threshold": 0.64,
                  "left_node_id": null,
                  "right_node_id": null
                }
              ]
            }
          ]
        }
      }

.. _alt-ext-api-ana-par-show:

分析パラメーター情報を取得する
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. http:get:: /analyses/[analysis_id]/parameter

   - レスポンスボディ

     - :ref:`alt-ext-res-ana-parameter`

   - ステータスコード

     - 成功時

       - 200

     - 失敗時

       - 404

   **リクエスト例**

   .. sourcecode:: http

      GET /analyses/15f61ba31273e7c342dd0934f894f0a0/parameter HTTP/1.1

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      {
        "max_depth": null,
        "max_features": "sqrt",
        "max_leaf_nodes": null,
        "min_samples_leaf": 1,
        "min_samples_split": 2,
        "num_tree": 10
      }

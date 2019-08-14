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

         - 不正な値を指定した場合は無視する

       - page

         - 指定したページの予測ジョブを返却する
         - デフォルト 1
         - 最大ページより大きい数を指定した場合は空配列を返却する

       - per_page

         - 指定した数の予測ジョブを返却する
         - デフォルト 10
         - 以下の場合，返却する数は指定した数よりも少なくなる可能性がある

           - pageパラメーターで最終ページを指定していた場合
           - 指定した数の予測ジョブ情報が登録されていない場合

   - レスポンスボディ

     - predictions

       - :ref:`reg-ext-res-prediction` の配列

   - ステータスコード

     - 成功時

       - 200

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
            "execute_at": "2019-08-02 09:00:00",
            "model": "model.zip",
            "from": "2019-08-01 00:00:00",
            "to": "2019-08-01 23:59:59",
            "pair": "USDJPY",
            "means": "auto",
            "result": "up",
            "state": "completed"
          }
        ]
      }

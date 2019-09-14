統計情報管理API
===============

本APIでは以下の機能を提供する

- :ref:`alg-ext-api-set-period`
- :ref:`alg-ext-api-set-category`

.. _alg-ext-api-set-period:

期間別収支を取得する
^^^^^^^^^^^^^^^^^^^^

.. http:get:: /settlements/period

   - リクエストクエリ

     - 必須

       - interval (string)

         - 集計間隔
         - 以下のいずれかを指定可能

           - yearly: 年単位で計算する
           - monthly: 月単位で計算する
           - daily: 日単位で計算する

   - レスポンスボディ

     - settlements

       - 以下のパラメーターの配列

         - date (string)

           - 集計した期間

         - price (integer)

           - 収支

   - ステータスコード

     - 成功時

       - 200

     - 失敗時

       - 400

   **リクエスト例**

   .. sourcecode:: http

      GET /settlement/period?interval=monthly HTTP/1.1

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      {
        "settlements": [
          {
            "date": "1000-01",
            "price": 200000
          }
        ]
      }

.. _alg-ext-api-set-category:

カテゴリ別収支を取得する
^^^^^^^^^^^^^^^^^^^^^^^^

.. http:get:: /settlements/category

   - リクエストクエリ

     - オプション

       - payment_type (string)

         - 計算に利用する収支の種類
         - :ref:`alg-ext-res-payment` - payment_type を参照

   - レスポンスボディ

     - settlements

       - 以下のパラメーターの配列

         - category (string)

           - カテゴリ

         - price (integer)

           - 収支

   - ステータスコード

     - 成功時

       - 200

     - 失敗時

       - 400

   **リクエスト例**

   .. sourcecode:: http

      GET /settlement/category?payment_type=income HTTP/1.1

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      {
        "settlements": [
          {
            "category": "給料",
            "price": 200000
          },
          {
            "category": "娯楽費",
            "price": 14000
          }
        ]
      }

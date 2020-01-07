収支情報管理API
===============

本APIでは以下の機能を提供する

- :ref:`alg-ext-api-pay-create`
- :ref:`alg-ext-api-pay-read`
- :ref:`alg-ext-api-pay-index`
- :ref:`alg-ext-api-pay-update`
- :ref:`alg-ext-api-pay-delete`

.. _alg-ext-api-pay-create:

収支を登録する
^^^^^^^^^^^^^^

.. http:post:: /payments

   - リクエストボディ

     - 必須

       - payment_type

         - :ref:`alg-ext-res-payment` のpayment_type参照

       - date

         - :ref:`alg-ext-res-payment` のdate参照

       - content

         - :ref:`alg-ext-res-payment` のcontent参照

       - categories (array[string])

         - :ref:`alg-ext-res-payment` のcategories参照
         - :ref:`alg-ext-res-category` の名前の配列

       - tags (array[string])

         - :ref:`alg-ext-res-payment` のtags参照
         - :ref:`alg-ext-res-tag` のタグ名の配列

       - price

         - :ref:`alg-ext-res-payment` のprice参照

   - レスポンスボディ

     - :ref:`alg-ext-res-payment`

   - ステータスコード

     - 成功時

       - 201

     - 失敗時

       - 400

   **リクエスト例**

   .. sourcecode:: http

      POST /payments HTTP/1.1
      Content-Type: application/json

      {
        "payment_type": "income",
        "date": "1000-01-01",
        "content": "給料",
        "categories": [
          "給料"
        ],
        "tags": [
          "給料"
        ],
        "price": 200000
      }

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 201 Created
      Content-Type: application/json

      {
        "id": 1,
        "payment_type": "income",
        "date": "1000-01-01",
        "content": "給料",
        "categories": [
          {
            "id": 1,
            "name": "給料",
            "description": null
          }
        ],
        "tags": [
          {
            "tag_id": "2d44e728b365a0c8f91987c39117cc08",
            "name": "給料"
          }
        ],
        "price": 200000
      }

.. _alg-ext-api-pay-read:

収支を取得する
^^^^^^^^^^^^^^

.. http:get:: /payments/[id]

   - パスパラメーター

     - id

       - :ref:`alg-ext-res-payment` のid参照

   - レスポンスボディ

     - :ref:`alg-ext-res-payment`

   - ステータスコード

     - 成功時

       - 200

     - 失敗時

       - 404

   **リクエスト例**

   .. sourcecode:: http

      GET /payments/1 HTTP/1.1

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      {
        "id": 1,
        "payment_type": "income",
        "date": "1000-01-01",
        "content": "給料",
        "categories": [
          {
            "id": 1,
            "name": "給料",
            "description": null
          }
        ],
        "tags": [
          {
            "tag_id": "2d44e728b365a0c8f91987c39117cc08",
            "name": "給料"
          }
        ],
        "price": 200000
      }

.. _alg-ext-api-pay-index:

収支を検索する
^^^^^^^^^^^^^^

.. http:get:: /payments

   - リクエストクエリ

     - オプション

       - payment_type

         - :ref:`alg-ext-res-payment` のpayment_type参照

       - date_before (string)

         - 指定された日付以前の収支を検索する

       - date_after (string)

         - 指定された日付以降の収支を検索する

       - content_equal (string)

         - 内容が完全に一致する収支を検索する

       - content_include (string)

         - 内容が部分的に一致する収支を検索する

       - category (string)

         - カテゴリが一致する収支を検索する

       - price_upper (string)

         - 指定された金額以上の収支を検索する

       - price_lower (string)

         - 指定された金額以下の収支を検索する

       - page (string)

         - 指定したページの収支を返却する
         - デフォルト 1
         - 最大ページより大きい数を指定した場合は空配列を返却する

       - per_page (string)

         - 指定した数の収支を返却する
         - デフォルト 10
         - 以下の場合，返却する数は指定した数よりも少なくなる可能性がある

           - pageパラメーターで最終ページを指定していた場合
           - 指定した数の収支情報が登録されていない場合

       - sort (string)

         - 指定したパラメーターで並べ替えて返却する
         - 以下を指定可能

           - id
           - date
           - price

         - デフォルト id

       - order (string)

         - 指定した順番で返却する
         - 以下を指定可能

           - asc: 昇順で返却する
           - desc: 降順で返却する

         - デフォルト asc

   - レスポンスボディ

     - payments

       - :ref:`alg-ext-res-payment` の配列

   - ステータスコード

     - 成功時

       - 200

     - 失敗時

       - 400

   **リクエスト例**

   .. sourcecode:: http

      GET /payments?payment_type=income HTTP/1.1

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      {
        "payments": [
          {
            "id": 1,
            "payment_type": "income",
            "date": "1000-01-01",
            "content": "給料",
            "categories": [
              {
                "id": 1,
                "name": "給料",
                "description": null
              }
            ],
            "tags": [
              {
                "tag_id": "2d44e728b365a0c8f91987c39117cc08",
                "name": "給料"
              }
            ],
            "price": 200000
          }
        ]
      }

.. _alg-ext-api-pay-update:

収支を更新する
^^^^^^^^^^^^^^

.. http:put:: /payments/[id]

   - パスパラメーター

     - id

       - :ref:`alg-ext-res-payment` のid参照

   - リクエストボディ

     - オプション

       - payment_type

         - :ref:`alg-ext-res-payment` のpayment_type参照

       - date

         - :ref:`alg-ext-res-payment` のdate参照

       - content

         - :ref:`alg-ext-res-payment` のcontent参照

       - categories

         - :ref:`alg-ext-res-payment` のcategories参照
         - :ref:`alg-ext-res-category` の名前の配列

       - price

         - :ref:`alg-ext-res-payment` のprice参照

   - レスポンスボディ

     - 更新後の :ref:`alg-ext-res-payment`

   - ステータスコード

     - 成功時

       - 200

     - 失敗時

       - 400
       - 404

   **リクエスト例**

   .. sourcecode:: http

      PUT /payments/1 HTTP/1.1
      Content-Type: application/json

      {
        "date": "1000-01-02"
      }

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      {
        "id": 1,
        "payment_type": "income",
        "date": "1000-01-02",
        "content": "給料",
        "categories": [
          {
            "id": 1,
            "name": "給料",
            "description": null
          }
        ],
        "tags": [
          {
            "tag_id": "2d44e728b365a0c8f91987c39117cc08",
            "name": "給料"
          }
        ],
        "price": 200000
      }

.. _alg-ext-api-pay-delete:

収支を削除する
^^^^^^^^^^^^^^

.. http:delete:: /payments/[id]

   - パスパラメーター

     - id

       - :ref:`alg-ext-res-payment` のid参照

   - ステータスコード

     - 成功時

       - 204

     - 失敗時

       - 404

   **リクエスト例**

   .. sourcecode:: http

      DELETE /payments/1 HTTP/1.1

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 204 No Content

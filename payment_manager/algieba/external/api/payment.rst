収支情報管理API
===============

本APIでは以下の機能を提供する

- :ref:`alg-ext-api-pay-create`
- :ref:`alg-ext-api-pay-read`
- :ref:`alg-ext-api-pay-index`
- :ref:`alg-ext-api-pay-update`
- :ref:`alg-ext-api-pay-delete`
- :ref:`alg-ext-api-pay-settle`

.. _alg-ext-api-pay-create:

収支を登録する
^^^^^^^^^^^^^^

.. http:post:: /payments

   :jsonparam string payment_type: ``income`` または ``expense``
   :jsonparam string date: 所持金の増減があった日時
   :jsonparam string content: 所持金の増減があった理由など
   :jsonparam string category: 費目（例：食費，水道光熱費）
   :jsonparam int price: 所持金の増減量

   :response JSONObject:
      - :ref:`alg-ext-res-payment`

        - id
        - payment_type
        - date
        - content
        - categories - :ref:`alg-ext-res-category` の配列

          - id
          - name
          - description

        - price

   :status 201:
      - 収支の登録に成功
      - :ref:`alg-ext-res-payment` を返す
   :status 400:
      - 収支の登録に失敗
      - :ref:`alg-ext-api-common-error` を返す

   **リクエスト例**

   .. sourcecode:: http

      POST /payments HTTP/1.1
      Content-Type: application/json

      {
        "payment_type": "income",
        "date": "1000-01-01",
        "content": "給料",
        "category": "給料",
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
        "price": 200000
      }

.. _alg-ext-api-pay-read:

収支を取得する
^^^^^^^^^^^^^^

.. http:get:: /payments/[id]

   :response JSONObject:
      - :ref:`alg-ext-res-payment`

        - id
        - payment_type
        - date
        - content
        - categories - :ref:`alg-ext-res-category` の配列

          - id
          - name
          - description

        - price

   :status 200:
      - 収支の取得に成功
      - :ref:`alg-ext-res-payment` を返す
   :status 404:
      - 収支の取得に失敗
      - 存在しないIDを指定

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
        "price": 200000
      }

.. _alg-ext-api-pay-index:

収支を検索する
^^^^^^^^^^^^^^

.. http:get:: /payments

   :query payment_type: ``income`` または ``expense``
   :query date_before: 指定された日付以前の収支を検索する
   :query date_after: 指定された日付以降の収支を検索する
   :query content_equal: 内容が完全に一致する収支を検索する
   :query content_include: 内容が部分的に一致する収支を検索する
   :query category: カテゴリが一致する収支を検索する
   :query price_upper: 指定された金額以上の収支を検索する
   :query price_lower: 指定された金額以下の収支を検索する
   :query page: 指定したページの収支を返却する
      - デフォルト 1
      - 最大ページより大きい数を指定した場合は空配列を返却する
   :query per_page: 指定した数の収支を返却する
      - デフォルト 10
      - 以下の場合，返却する数は指定した数よりも少なくなる可能性がある

        - ``page`` パラメーターで最終ページを指定していた場合
        - 指定した数の収支情報が登録されていない場合
   :query sort: 指定したパラメーターで並べ替えて返却する
      - 以下を指定可能

        - id
        - date
        - price
      - デフォルト id
   :query order: 指定した順番で返却する
      - 以下を指定可能

        - asc: 昇順で返却する
        - desc: 降順で返却する
      - デフォルト asc

   :responseArray JSONObject:
      - :ref:`alg-ext-res-payment` の配列

        - id
        - payment_type
        - date
        - content
        - categories - :ref:`alg-ext-res-category` の配列

          - id
          - name
          - description

        - price

   :status 200:
      - 収支の検索に成功
      - :ref:`alg-ext-res-payment` の配列を返す
   :status 400:
      - 収支の検索に失敗
      - :ref:`alg-ext-api-common-error` を返す

   **リクエスト例**

   .. sourcecode:: http

      GET /payments?payment_type=income HTTP/1.1

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      [
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
          "price": 200000
        }
      ]

.. _alg-ext-api-pay-update:

収支を更新する
^^^^^^^^^^^^^^

.. http:put:: /payments/[id]

   :request JSONObject:
      - 更新する :ref:`alg-ext-res-payment` の属性と更新値

   :response JSONObject:
      - :ref:`alg-ext-res-payment`

        - id
        - payment_type
        - date
        - content
        - categories - :ref:`alg-ext-res-category` の配列

          - id
          - name
          - description

        - price

   :status 201:
      - 収支の更新に成功
      - :ref:`alg-ext-res-payment` を返す
   :status 400:
      - 収支の更新に失敗
      - :ref:`alg-ext-api-common-error` を返す
   :status 404:
      - 収支の更新に失敗
      - 存在しないIDを指定

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
        "price": 200000
      }

.. _alg-ext-api-pay-delete:

収支を削除する
^^^^^^^^^^^^^^

.. http:delete:: /payments/[id]

   :status 204:
      - 収支の削除に成功
   :status 404:
      - 収支の削除に失敗

   **リクエスト例**

   .. sourcecode:: http

      DELETE /payments/1 HTTP/1.1

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 204 No Content

.. _alg-ext-api-pay-settle:

収支を計算する
^^^^^^^^^^^^^^

.. http:get:: /settlement

   :query interval:
      - 集計間隔
      - ``yearly``, ``monthly``, ``daily`` のいずれかを指定

   :status 200:
      - 収支の計算に成功
   :status 400:
      - 収支の計算に失敗
      - :ref:`alg-ext-api-common-error` を返す

   **リクエスト例**

   .. sourcecode:: http

      GET /settlement?interval=monthly HTTP/1.1

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      [
        {
          "date": "1000-01",
          "price": 200000
        }
      ]

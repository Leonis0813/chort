辞書情報管理API
===============

本APIでは以下の機能を提供する

- :ref:`alg-ext-api-dic-create`
- :ref:`alg-ext-api-dic-index`

.. _alg-ext-api-dic-create:

辞書を登録する
^^^^^^^^^^^^^^

.. http:post:: /dictionaries

   :<json string phrase: :ref:`alg-ext-res-dictionary` のフレーズ参照
   :<json string condition: :ref:`alg-ext-res-dictionary` の条件参照
   :<json string categories: :ref:`alg-ext-res-category` の配列

   :>json int id: :ref:`alg-ext-res-dictionary` のID参照
   :>json string phrase: :ref:`alg-ext-res-dictionary` のフレーズ参照
   :>json string condition: :ref:`alg-ext-res-dictionary` の条件参照
   :>json object categories: :ref:`alg-ext-res-category` の配列
      :>json int id: :ref:`alg-ext-res-category` のID参照
      :>json string name: :ref:`alg-ext-res-category` の名前参照
      :>json string description: :ref:`alg-ext-res-category` の意味参照
   :status 201: 辞書の登録に成功
   :status 400: 辞書の登録に失敗
      - :ref:`alg-ext-api-common-error` を返す

   **リクエスト例**

   .. sourcecode:: http

      POST /dictionaries HTTP/1.1
      Content-Type: application/json

      {
        "phrase": "コンビニ",
        "condition": "include",
        "categories": [
          {
            "name": "食費"
          }
        ]
      }

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 201 Created
      Content-Type: application/json

      {
        "id": 1,
        "phrase": "コンビニ",
        "condition": "include",
        "categories": [
          {
            "id": 1,
            "name": "食費",
            "description": null
          }
        ]
      }

.. _alg-ext-api-dic-index:

辞書を検索する
^^^^^^^^^^^^^^

.. http:get:: /dictionaries

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
      - :ref:`alg-ext-resource-payment` の配列

        - id
        - payment_type
        - date
        - content
        - categories - :ref:`alg-ext-resource-category` の配列

          - id
          - name
          - description

        - price

   :status 200:
      - 収支の検索に成功
      - :ref:`alg-ext-resource-payment` の配列を返す
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

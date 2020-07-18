辞書情報管理API
===============

本APIでは以下の機能を提供する

- :ref:`alg-ext-api-dic-create`
- :ref:`alg-ext-api-dic-index`

.. _alg-ext-api-dic-create:

辞書を作成する
^^^^^^^^^^^^^^

.. http:post:: /dictionaries

   - リクエストボディ

     - 必須

       - phrase

         - :ref:`alg-ext-res-dictionary` のphrase参照

       - condition

         - :ref:`alg-ext-res-dictionary` のcondition参照

       - categories (array[string])

         - :ref:`alg-ext-res-dictionary` のcategories参照
         - :ref:`alg-ext-res-category` の名前の配列

   - レスポンスボディ

     - :ref:`alg-ext-res-dictionary`

   - ステータスコード

     - 成功時

       - 201

     - 失敗時

       - 400

   **リクエスト例**

   .. sourcecode:: http

      POST /dictionaries HTTP/1.1
      Content-Type: application/json

      {
        "phrase": "コンビニ",
        "condition": "include",
        "categories": [
          "食費"
        ]
      }

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 201 Created
      Content-Type: application/json

      {
        "dictionary_id": "2d44e728b365a0c8f91987c39117cc08",
        "phrase": "コンビニ",
        "condition": "include",
        "categories": [
          {
            "category_id": "2d44e728b365a0c8f91987c39117cc08",
            "name": "食費",
            "description": null
          }
        ]
      }

.. _alg-ext-api-dic-index:

辞書を検索する
^^^^^^^^^^^^^^

.. http:get:: /dictionaries

   - リクエストクエリ

     - オプション

       - condition

         - :ref:`alg-ext-res-dictionary` のcondition参照
         - 条件が一致する辞書を検索する

       - content

         - :ref:`alg-ext-res-payment` のcontent参照
         - フレーズと条件を満たす内容となるような辞書を検索する

       - phrase

         - :ref:`alg-ext-res-dictionary` のphrase参照
         - フレーズが一致する辞書を検索する

   - レスポンスボディ

     - dictionaries

       - :ref:`alg-ext-res-dictionary` の配列

   - ステータスコード

     - 成功時

       - 200

     - 失敗時

       - 400

   **リクエスト例**

   .. sourcecode:: http

      GET /dictionaries?content=コンビニで食べ物購入 HTTP/1.1

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      {
        "dictionaries": [
          {
            "dictionary_id": "2d44e728b365a0c8f91987c39117cc08",
            "phrase": "コンビニ",
            "condition": "include",
            "categories": [
              {
                "category_id": "2d44e728b365a0c8f91987c39117cc08",
                "name": "食費",
                "description": null
              }
            ]
          }
        ]
      }

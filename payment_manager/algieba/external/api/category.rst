カテゴリ情報管理API
===================

本APIでは以下の機能を提供する

- :ref:`alg-ext-api-cat-index`

.. _alg-ext-api-cat-index:

カテゴリを検索する
^^^^^^^^^^^^^^^^^^

.. http:get:: /categories

   - リクエストクエリ

     - オプション

       - keyword (string)

         - keywordを含むカテゴリを検索する
         - 指定しなかった場合は全てのカテゴリを取得する

   - レスポンスボディ

     - categories

       - :ref:`alg-ext-res-category` の配列

   - ステータスコード

     - 成功時

       - 200

     - 失敗時

       - 400

   **リクエスト例**

   .. sourcecode:: http

      GET /categories?keyword=食費 HTTP/1.1

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      {
        "categories": [
          {
            "id": 1,
            "name": "食費",
            "description": "食品や飲料を購入した時に発生する支出"
          }
        ]
      }

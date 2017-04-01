カテゴリ情報管理API
===================

本APIでは以下の機能を提供する

- :ref:`alg-ext-api-cat-index`

.. _alg-ext-api-cat-index:

カテゴリを検索する
^^^^^^^^^^^^^^^^^^

.. http:get:: /categories

   :query keyword: keywordを含むカテゴリを検索する

   :responseArray JSONObject:
      - :ref:`alg-ext-resource-category`

        - id
        - name
        - description

   :status 200:
      - カテゴリの検索に成功
      - :ref:`alg-ext-resource-category` の配列を返す
   :status 400:
      - カテゴリの検索に失敗
      - :ref:`alg-ext-api-common-error` を返す

   **リクエスト例**

   .. sourcecode:: http

      GET /categories?keyword=食費 HTTP/1.1

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      [
        {
          "id": 1,
          "name": "食費",
          "description": "食品や飲料を購入した時に発生する支出"
        }
      ]

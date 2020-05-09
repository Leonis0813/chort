タグ情報管理API
===============

本APIでは以下の機能を提供する

- :ref:`alg-ext-api-tag-create`

.. _alg-ext-api-tag-create:

タグを作成する
^^^^^^^^^^^^^^

.. http:post:: /tags

   - リクエストボディ

     - 必須

       - name

         - :ref:`alg-ext-res-tag` のname参照

   - レスポンスボディ

     - :ref:`alg-ext-res-tag`

   - ステータスコード

     - 成功時

       - 201

     - 失敗時

       - 400

   **リクエスト例**

   .. sourcecode:: http

      POST /tags HTTP/1.1
      Content-Type: application/json

      {
        "name": "コンビニ"
      }

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 201 Created
      Content-Type: application/json

      {
        "tag_id": "2d44e728b365a0c8f91987c39117cc08",
        "name": "コンビニ"
      }

タグ情報管理API
===============

本APIでは以下の機能を提供する

- :ref:`alg-ext-api-tag-create`
- :ref:`alg-ext-api-tag-assign`

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

.. _alg-ext-api-tag-assign:

タグを設定する
^^^^^^^^^^^^^^

.. http:post:: /tags/[tag_id]/payments

   - パスパラメーター

     - tag_id

       - :ref:`alg-ext-res-tag` のtag_id参照

   - リクエストボディ

     - 必須

       - payment_ids (array[string])

         - :ref:`alg-ext-res-payment` のpayment_id参照

   - レスポンスボディ

     - なし

   - ステータスコード

     - 成功時

       - 200

     - 失敗時

       - 400

   **リクエスト例**

   .. sourcecode:: http

      POST /tags/2d44e728b365a0c8f91987c39117cc08/payments HTTP/1.1
      Content-Type: application/json

      {
        "payment_ids": [
          "2d44e728b365a0c8f91987c39117cc08"
        ]
      }

   **レスポンス例**

   .. sourcecode:: http

      HTTP/1.1 200 OK

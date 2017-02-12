機能仕様
========

機能仕様では以下を定義する

- :ref:`alg-ext-resource`
- :ref:`alg-ext-ui`
- :ref:`alg-ext-api`

.. _alg-ext-resource:

リソース
--------

本システムでは以下のリソースを扱う

- :ref:`alg-ext-resource-user`
- :ref:`alg-ext-resource-application`
- :ref:`alg-ext-resource-payment`

.. _alg-ext-resource-user:

ユーザーリソース
^^^^^^^^^^^^^^^^

本アプリの利用者を表す

.. csv-table::
   :header: "属性名", "型", "意味", "フォーマット", "備考"
   :widths: 10, 10, 20, 20, 40

   "ユーザーID", "文字列(string)", "ユーザーを識別する文字列", "半角英数字",
   "パスワード", "文字列(string)", "ユーザー認証を行うための鍵", "半角英数字",

.. _alg-ext-resource-application:

アプリリソース
^^^^^^^^^^^^^^

本アプリを利用するアプリを表す

.. csv-table::
   :header: "属性名", "型", "意味", "フォーマット", "備考"
   :widths: 10, 10, 20, 20, 40

   "アプリID", "文字列(string)", "アプリを識別する文字列", "- 16文字
   - 半角英数字", "本アプリによって発行される"
   "アプリキー", "文字列(string)", "アプリが持つ秘密鍵", "- 16文字
   - 半角英数字", "本アプリによって発行される"

.. _alg-ext-resource-payment:

収支リソース
^^^^^^^^^^^^

所持金の増減を表す

.. csv-table::
   :header: "入力項目", "型", "意味", "フォーマット", "備考"
   :widths: 10, 10, 20, 20, 40

   "種類", "文字列(string)", "収入, 支出を表す文字列", "``income`` または ``expense``",
   "日付", "文字列(string)", "所持金の増減があった日時", "yyyy-mm-dd",
   "内容", "文字列(string)", "所持金の増減があった理由など", "任意の文字列",
   "カテゴリ", "文字列(string)", "費目（例：食費，水道光熱費）", "任意の文字列",
   "金額", "自然数(integer)", "所持金の増減量", "半角数字",

.. _alg-ext-ui:

ユーザーインターフェース
------------------------

利用者はブラウザから収支の登録や確認を行うことができる

認証画面
^^^^^^^^

.. image:: images/login.jpg
   :alt: 認証画面

- 画面中央部に入力フォームが表示される
- ユーザーID, パスワードを入力して、ログインボタンを押すと認証が行われる
- 認証に成功すると管理画面に遷移する

管理画面
^^^^^^^^

.. image:: images/management.jpg
   :alt: 管理画面

- 画面の上部に登録用の入力フォームが表示される
- リセットボタンを押すと，入力フォームが全て空欄になる
- 入力フォームの下には表形式で収支の一覧が表示される
- 収支情報はページングされており，全件数と下記ページへのリンクが表示されている

  - 先頭ページ
  - 最終ページ
  - 次ページ
  - 前ページ
  - 表示中のページから前後4ページ

- 最新の収支から順番に表示される
- 1ページ50件の収支が表示される
- 登録成功時，画面遷移なしで表に登録した収支が表示される
- 収支情報の右側にあるボタンを押すと、対応する収支情報が削除される

管理画面（登録失敗時）
^^^^^^^^^^^^^^^^^^^^^^

.. image:: images/management_failure.jpg

- 登録失敗時，失敗した入力項目をポップアップで通知する

.. _alg-ext-api:

Web API
-------

以下のAPIを定義する

- :ref:`alg-ext-api-create`
- :ref:`alg-ext-api-read`
- :ref:`alg-ext-api-index`
- :ref:`alg-ext-api-update`
- :ref:`alg-ext-api-delete`
- :ref:`alg-ext-api-settle`

共通定義
^^^^^^^^

.. _alg-ext-api-common-error:

エラーコード
""""""""""""

.. csv-table::
   :header: "エラーコード", "ステータスコード", "意味"

   "absent_param_[属性]", "400", "入力必須の項目がない"
   "invalid_param_[属性]", "400", "不正値のパラメータがある"

.. _alg-ext-api-create:

収支を登録する
^^^^^^^^^^^^^^

.. http:post:: /payments

   :jsonparam string payment_type: ``income`` または ``expense``
   :jsonparam string date: 所持金の増減があった日時
   :jsonparam string content: 所持金の増減があった理由など
   :jsonparam string category: 費目（例：食費，水道光熱費）
   :jsonparam int price: 所持金の増減量

   :response JSONObject:
      - :ref:`alg-ext-resource-payment`

        - id
        - payment_type
        - date
        - content
        - category
        - price
        - created_at
        - updated_at

   :status 201:
      - 収支の登録に成功
      - :ref:`alg-ext-resource-payment` を返す
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
        "category": "給料",
        "price": 200000,
        "created_at": "1000-01-01 00:00:00",
        "updated_at": "1000-01-01 00:00:00"
      }

.. _alg-ext-api-read:

収支を取得する
^^^^^^^^^^^^^^

.. http:get:: /payments/[id]

   :response JSONObject:
      - :ref:`alg-ext-resource-payment`

        - id
        - payment_type
        - date
        - content
        - category
        - price
        - created_at
        - updated_at

   :status 200:
      - 収支の取得に成功
      - :ref:`alg-ext-resource-payment` を返す
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
        "category": "給料",
        "price": 200000,
        "created_at": "1000-01-01 00:00:00",
        "updated_at": "1000-01-01 00:00:00"
      }

.. _alg-ext-api-index:

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

   :responseArray JSONObject:
      - :ref:`alg-ext-resource-payment`

        - id
        - payment_type
        - date
        - content
        - category
        - price
        - created_at
        - updated_at

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
          "category": "給料",
          "price": 200000,
          "created_at": "1000-01-01 00:00:00",
          "updated_at": "1000-01-01 00:00:00"
        }
      ]

.. _alg-ext-api-update:

収支を更新する
""""""""""""""

.. http:put:: /payments/[id]

   :request JSONObject:
      - 更新する :ref:`alg-ext-resource-payment` の属性と更新値

   :response JSONObject:
      - :ref:`alg-ext-resource-payment`

        - id
        - payment_type
        - date
        - content
        - category
        - price
        - created_at
        - updated_at

   :status 201:
      - 収支の更新に成功
      - :ref:`alg-ext-resource-payment` を返す
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
        "category": "給料",
        "price": 200000,
        "created_at": "1000-01-01 00:00:00",
        "updated_at": "1000-01-01 00:00:00"
      }

.. _alg-ext-api-delete:

収支を削除する
""""""""""""""

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

.. _alg-ext-api-settle:

収支を計算する
""""""""""""""

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

      {
        "1000-01": 200000
      }

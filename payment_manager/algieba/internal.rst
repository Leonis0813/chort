設計仕様
========

設計仕様では以下を定義する

- :ref:`alg-int-cls`
- :ref:`alg-int-seq`
- :ref:`alg-int-scm`

.. _alg-int-cls:

モジュール構成
--------------

MVCモデルを利用する

*クラス図*

.. uml:: umls/class.uml

- Model

  - Category

    - categoriesテーブルを操作するモデル

  - Payment

    - paymentsテーブルを操作するモデル

  - Query

    - 収支検索時のクエリを管理するモデル

  - Settlement

    - 収支計算時のクエリを管理するモデル

  - Dictionary

    - dictionariesテーブルを管理するモデル

- View

  - Payment_View

    - 収支の登録や表示を行うビュー

  - Statistics_View

    - 統計情報を表示するビュー

- Controller

  - Api::CategoriesController

    - カテゴリを処理するコントローラー
    - WebAPI用コントローラー

  - Api::DictionariesController

    - 辞書を処理するコントローラー
    - WebAPI用コントローラー

  - Api::PaymentsController

    - 収支を処理するコントローラー
    - WebAPI用コントローラー

  - PaymentsController

    - 収支を処理するコントローラー
    - UI用コントローラー

  - StatisticsController

    - 統計情報を管理するコントローラー
    - UI用コントローラー

.. _alg-int-seq:

シーケンス
----------

- :ref:`alg-int-seq-create-payment`
- :ref:`alg-int-seq-index-payment`
- :ref:`alg-int-seq-destroy-payment`
- :ref:`alg-int-seq-settle-payment`
- :ref:`alg-int-seq-show-stats`
- :ref:`alg-int-seq-create-dictionary`

.. _alg-int-seq-create-payment:

収支を登録する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-create-payment.uml

1. 利用者が管理画面を表示する
2. ブラウザが管理画面の表示を要求する
3. 利用者が内容を入力する
4. 管理画面が辞書情報を検索するAPIを実行する
5. 入力された内容にマッチする辞書情報を検索する
6. 利用者がフォームに入力して登録ボタンを押下する
7. 管理画面が収支情報を登録するAPIを実行する

必須パラメーターがない場合はエラーを表示して終了する

8. 入力されたパラメーターから収支情報を作成する

入力されたカテゴリの数だけ9を実行する

9. カテゴリ情報が登録されていなければ登録し，取得する

10. 収支情報をDBに登録する

登録に成功した場合は11を実行する

11. 管理画面をリロードする

登録に失敗した場合はエラーを表示して終了する

.. _alg-int-seq-index-payment:

収支を検索する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-index-payment.uml

1. 利用者がフォームに入力して検索フォームを押下する
2. クエリを指定して登録画面の再表示を要求する
3. 入力されたパラメーターからクエリ情報を作成する
4. クエリ情報が不正でないか確認する

クエリ情報が不正な場合はエラーを表示して終了する

5. クエリを満たす収支情報を検索する

.. _alg-int-seq-destroy-payment:

収支を削除する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-destroy-payment.uml

1. 利用者が収支情報を選択して削除ボタンを押下する
2. 登録画面が収支情報を削除するAPIを実行する
3. 指定された収支情報が存在すれば削除する
4. 登録画面をリロードする

.. _alg-int-seq-settle-payment:

収支を計算する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-settle.uml

1. クライアントが収支を計算するAPIを実行する
2. 指定されたパラメーターから決済情報を作成する
3. 決済情報が不正でないか確認する

決済情報が不正な場合はエラーコードを返して終了する

4. 収支情報から収支を計算する

.. _alg-int-seq-show-stats:

統計情報を表示する
^^^^^^^^^^^^^^^^^^

.. uml:: umls/seq-show-stats.uml

1. 利用者が統計画面を表示する
2. ブラウザが統計画面の表示を要求する
3. 統計画面が月次で収支を計算するAPIを実行する
4. 収支情報から収支を計算する
5. 利用者がグラフをクリックする
6. 統計画面が日次で収支を計算するAPIを実行する
7. 収支情報から収支を計算する

.. _alg-int-seq-create-dictionary:

辞書を登録する
^^^^^^^^^^^^^^

.. uml:: umls/seq-create-dictionary.uml

1. 利用者がフォームに入力して登録ボタンを押下する
2. 管理画面が辞書情報を登録するAPIを実行する

必須パラメーターがない場合はエラーを表示して終了する

3. 入力されたパラメーターから辞書情報を作成する
4. 辞書情報をDBに登録する

.. _alg-int-scm:

データベース構成
----------------

データベースは下記のテーブルで構成される

- :ref:`alg-int-scm-categories`
- :ref:`alg-int-scm-categories-payments`
- :ref:`alg-int-scm-payments`

.. _alg-int-scm-categories:

categories テーブル
^^^^^^^^^^^^^^^^^^^

カテゴリを登録するcategoriesテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"

   "id", "INTEGER", "categoryオブジェクトのID", "◯", "◯"
   "name", "STRING", "カテゴリの名前",, "◯"
   "description", "STRING", "カテゴリの説明",,
   "created_at", "DATETIME", "カテゴリ情報が登録された日時",, "◯"
   "updated_at", "DATETIME", "カテゴリ情報が登録 or 更新された日時",, "◯"

.. _alg-int-scm-categories-payments:

categories_payments テーブル
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

カテゴリと収支情報を紐づける中間テーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"

   "category_id", "INTEGER", "categoryオブジェクトのID", "◯", "◯"
   "payment_id", "INTEGER", "paymentオブジェクトのID", "◯", "◯"

.. _alg-int-scm-payments:

payments テーブル
^^^^^^^^^^^^^^^^^

収支を登録するpaymentsテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"

   "id", "INTEGER", "paymentオブジェクトのID", "◯", "◯"
   "payment_type", "STRING", "収入/支出を表すフラグ",, "◯"
   "date", "DATE", "収入/支出があった日",, "◯"
   "content", "STRING", "収入/支出の内容",, "◯"
   "price", "INTEGER", "収入/支出の金額",, "◯"
   "created_at", "DATETIME", "収支が登録された日時",, "◯"
   "updated_at", "DATETIME", "収支が登録 or 更新された日時",, "◯"

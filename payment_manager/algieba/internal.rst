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

1. リクエストを受けると，PaymentsControllerクラスのcreateメソッドを実行する

   - 必須パラメーターがない場合

     - BadRequestを発生させてステータスコード400とエラーコードを返す

2. Categoryクラスのfind_or_create_byメソッドを実行してcategoryパラメーターで指定されたカテゴリを取得し，存在しなければ作成する
3. Paymentクラスのcreateメソッドを実行して収支情報を作成する

   - 作成に成功した場合

     - ステータスコード201と登録したPaymentオブジェクトを返す

   - 作成に失敗した場合

     - BadRequestを発生させて，ステータスコード400とエラーコードを返す

.. _alg-int-seq-index-payment:

収支を検索する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-index-payment.uml

1. リクエストを受けると，PaymentsControllerクラスのindexメソッドを実行する
2. パラメーターからQueryクラスのオブジェクトを作成する
3. valid?メソッドを実行して不正な値がないかチェックする

   - 不正な値がある場合

     - BadRequestを発生させて，ステータスコード400とエラーコードを返す

4. whereメソッドを実行してPaymentオブジェクトの配列を取得する

   - ステータスコード200と取得したPaymentオブジェクトの配列を返す

.. _alg-int-seq-destroy-payment:

収支を削除する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-destroy-payment.uml

1. リクエストを受けると，PaymentsControllerクラスのdestroyメソッドを実行する
2. Paymentクラスのdestroyメソッドを実行して削除する

   - 削除に成功した場合

     - ステータスコード200と取得したPaymentオブジェクトを返す

   - 削除に失敗した場合

     - NotFoundを発生させて，ステータスコード404とエラーコードを返す

.. _alg-int-seq-settle-payment:

収支を計算する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-settle.uml

1. リクエストを受けると，PaymentsControllerクラスのsettleメソッドを実行する
2. パラメーターからSettlementクラスのオブジェクトを作成する
3. valid?メソッドを実行して不正な値がないかチェックする

   - "daily", "monthly", "yearly"以外の場合

     - BadRequestを発生させて，ステータスコード400とエラーコードを返す

4. settleメソッドを実行して収支を計算する

   - ステータスコード200と計算結果を返す

.. _alg-int-seq-show-stats:

統計情報を表示する
^^^^^^^^^^^^^^^^^^

.. uml:: umls/seq-show-stats.uml

1. 利用者が統計情報確認画面にアクセスする
2. StatisticsControllerのshowメソッドを実行し，画面を表示する
3. Statistics_ViewがApi::PaymentsControllerのsettleメソッドを実行し，収支をグラフで表示する
4. 利用者がグラフの棒をクリックする
5. Statics_ViewがApi::PaymentsControllerのsettleメソッドを実行し，日次の収支を取得する

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

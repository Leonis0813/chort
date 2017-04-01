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

  - Category: categoryテーブルを操作するモデル
  - Client: clientsテーブルを操作するモデル
  - Payment: paymentsテーブルを操作するモデル

    - settle: 収支を計算するメソッド

  - Query: 収支検索時のクエリを管理するモデル

    - date_valid?: 日付を検証するためのメソッド

  - Settlement: 収支計算時のクエリを管理するモデル
  - User: usersテーブルを操作するモデル

- View

  - Login_View: 認証を行うビュー

    - 利用者が未認証時に表示される

  - Payment_View: 収支の登録や表示を行うビュー

    - 認証された利用者が収支の登録・参照を行う

- Controller

  - CategoriesController: カテゴリを処理するコントローラ

    - index: カテゴリを検索するメソッド

  - LoginController: 認証処理を行うコントローラー

    - authenticate_user: ユーザー認証を行うメソッド
    - authenticate_client: アプリ認証を行うメソッド

  - PaymentsController: 収支を処理するコントローラ

    - manage: ブラウザに管理画面を表示するメソッド
    - create: 収支を登録するメソッド
    - read: 収支を取得するメソッド
    - index: 収支を検索するメソッド
    - update: 収支を更新するメソッド
    - delete: 収支を削除するメソッド
    - settle: 収支を計算するメソッド
    - payment_params: Paymentの属性名の配列を返すメソッド
    - index_params: Queryの属性名の配列を返すメソッド

.. _alg-int-seq:

シーケンス
----------

- :ref:`alg-int-seq-login`
- :ref:`alg-int-seq-create-payment`
- :ref:`alg-int-seq-read-payment`
- :ref:`alg-int-seq-index-payment`
- :ref:`alg-int-seq-update-payment`
- :ref:`alg-int-seq-delete-payment`
- :ref:`alg-int-seq-settle-payment`
- :ref:`alg-int-seq-index-category`

.. _alg-int-seq-login:

ログインする
^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-login.uml

1. 利用者がブラウザから本アプリにアクセスする
2. 利用者がユーザーIDとパスワードを入力してログインする
3. LoginControllerがユーザーIDとパスワードが一致するUserオブジェクトを検索する
4. 一致するユーザーが存在しなければLogin_Viewを表示して2へ戻る
5. 一致するユーザーが存在すればPaymentController#manageを実行する
6. PaymentControllerがPaymentを取得してPayment_Viewを表示する

.. _alg-int-seq-create-payment:

収支を登録する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-create-payment.uml

1. リクエストを受けると，PaymentsControllerクラスのcreateメソッドを実行する

   - 必須パラメーターがない場合

     - BadRequestを発生させてステータスコード400とエラーコードを返す

2. Categoryクラスのwhereメソッドを実行してcategoryパラメーターで指定されたカテゴリが存在するかチェックする
3. 指定されたカテゴリが存在しない場合，Categoryクラスのcreateメソッドを実行してカテゴリを作成する
4. Paymentクラスのcreateメソッドを実行して収支情報を作成する

   - 作成に成功した場合

     - ステータスコード201と登録したPaymentオブジェクトを返す

   - 作成に失敗した場合

     - BadRequestを発生させて，ステータスコード400とエラーコードを返す

.. _alg-int-seq-read-payment:

収支を取得する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-read-payment.uml

1. リクエストを受けると，PaymentsControllerクラスのreadメソッドを実行する
2. findメソッドでPaymentオブジェクトを取得する

   - 取得に成功した場合

     - ステータスコード200と取得したPaymentオブジェクトを返す

   - 取得に失敗した場合

     - NotFoundを発生させて，ステータスコード404とエラーコードを返す

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

.. _alg-int-seq-update-payment:

収支を更新する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-update-payment.uml

1. リクエストを受けると，PaymentsControllerクラスのupdateメソッドを実行する
2. update_attributesメソッドでPaymentオブジェクトを更新する

   - 不正な値がある場合

     - BadRequestを発生させて，ステータスコード400とエラーコードを返す

   - 不正な値がない場合

     - ステータスコード200と更新したPaymentオブジェクトを返す

.. _alg-int-seq-delete-payment:

収支を削除する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-delete-payment.uml

1. リクエストを受けると，PaymentsControllerクラスのdeleteメソッドを実行する
2. Paymentクラスのdeleteメソッドを実行して削除する

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

.. _alg-int-seq-index-category:

カテゴリを検索する
^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-index-category.uml

1. リクエストを受けると，CategoriesControllerクラスのindexメソッドを実行する
2. Categoryクラスのwhereメソッドを実行してカテゴリを検索する
3. ステータスコード200とCategoryオブジェクトの配列を返す

.. _alg-int-scm:

データベース構成
----------------

データベースは下記のテーブルで構成される

- :ref:`alg-int-scm-categories`
- :ref:`alg-int-scm-clients`
- :ref:`alg-int-scm-payments`
- :ref:`alg-int-scm-users`

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

.. _alg-int-scm-clients:

clients テーブル
^^^^^^^^^^^^^^^^

アプリを登録するclientsテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"

   "id", "INTEGER", "clientオブジェクトのID", "◯", "◯"
   "application_id", "STRING", "クライアントアプリのID",, "◯"
   "application_key", "STRING", "クライアントアプリのキー",, "◯"
   "created_at", "DATETIME", "アプリ情報が登録された日時",, "◯"
   "updated_at", "DATETIME", "アプリ情報が登録 or 更新された日時",, "◯"

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
   "category_id", "INTEGER", "categoryオブジェクトのID",, "◯"
   "price", "INTEGER", "収入/支出の金額",, "◯"
   "created_at", "DATETIME", "収支が登録された日時",, "◯"
   "updated_at", "DATETIME", "収支が登録 or 更新された日時",, "◯"

.. _alg-int-scm-users:

users テーブル
^^^^^^^^^^^^^^

ユーザーを登録するusersテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"

   "id", "INTEGER", "userオブジェクトのID", "◯", "◯"
   "user_id", "STRING", "ユーザーが登録したID",, "◯"
   "password", "STRING", "パスワード",, "◯"
   "created_at", "DATETIME", "ユーザー情報が登録された日時",, "◯"
   "updated_at", "DATETIME", "ユーザー情報が登録 or 更新された日時",, "◯"

設計仕様
========

設計仕様では以下を定義する

- :ref:`int-class`
- :ref:`int-sequence`
- :ref:`int-schema`

.. _int-class:

モジュール構成
--------------

MVCモデルを利用する

*クラス図*

.. uml::

   class Login_View <<Boundary>>
   class Payment_View <<Boundary>>

   class LoginController {
     + authenticate_user() ; void
     + authenticate_client() : void
   }

   class PaymentsController {
     + manage() : void
     + create(request_parameters : Hash) : void
     + read(id : String) : void
     + index(query_parameters : Hash) : void
     + update(id : String, request_parameters : Hash) : void
     + delete(id : String) : void
     + settle(interval : String) : void
     - payment_params() : Array
     - index_params() : Array
   }

   class User {
     + user_id : String
     + password : String
   }

   class Client {
     + application_id : String
     + application_key : String
   }

   class Payment {
     + payment_type : String
     + date : Date
     + content : String
     + category : String
     + price : Fixnum
     + settle(interval : String) : Hash<String, Fixnum>
   }

   class Query {
     - payment_type : String
     - date_before : String
     - date_after : String
     - content_equal : String
     - content_include : String
     - category : String
     - price_upper : int
     - price_lower : int
     + date_valid?() : void
   }

   class Settlement {
     - interval : String
   }

   Login_View -right- LoginController
   LoginController -- User
   LoginController -- Client
   Payment_View -right- PaymentsController
   PaymentsController "1" -- "0..*" Payment
   PaymentsController -- Query
   PaymentsController -- Settlement

- Model

  - User: usersテーブルを操作するモデル
  - Client: clientsテーブルを操作するモデル
  - Payment: paymentsテーブルを操作するモデル

    - settle: 収支を計算するメソッド

  - Query: 収支検索時のクエリを管理するモデル

    - date_valid?: 日付を検証するためのメソッド

  - Settlement: 収支計算時のクエリを管理するモデル

- View

  - Login_View: 認証を行うビュー

    - 利用者が未認証時に表示される

  - Payment_View: 収支の登録や表示を行うビュー

    - 認証された利用者が収支の登録・参照を行う

- Controller

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

.. _int-sequence:

シーケンス
----------

- :ref:`int-sequence-login`
- :ref:`int-sequence-create`
- :ref:`int-sequence-read`
- :ref:`int-sequence-index`
- :ref:`int-sequence-update`
- :ref:`int-sequence-delete`
- :ref:`int-sequence-settle`

.. _int-sequence-login:

ログインする
^^^^^^^^^^^^

*シーケンス図*

.. uml::

   autonumber

   actor 利用者
   利用者 -> Login_View
   Login_View -> LoginController : authenticate_user
   LoginController -> User : find

   autonumber stop
   User --> LoginController
   LoginController --> Login_View

   alt ログイン成功
     autonumber resume
     Login_View -> PaymentController : manage
     PaymentController -> Payment_View

     autonumber stop
     Payment_View --> 利用者
   end

1. 利用者がブラウザから本アプリにアクセスする
2. 利用者がユーザーIDとパスワードを入力してログインする
3. LoginControllerがユーザーIDとパスワードが一致するUserオブジェクトを検索する
4. 一致するユーザーが存在しなければLogin_Viewを表示して2へ戻る
5. 一致するユーザーが存在すればPaymentController#manageを実行する
6. PaymentControllerがPaymentを取得してPayment_Viewを表示する

.. _int-sequence-create:

収支を登録する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml::

   autonumber

   actor 利用者
   利用者 -> PaymentsController : create
   PaymentsController -> Payment : create

   autonumber stop
   Payment --> PaymentsController
   PaymentsController --> 利用者

1. リクエストを受けると，PaymentsControllerクラスのcreateメソッドを実行する
2. 必須パラメーターをチェックする

   - 必須パラメーターがない場合

     3-1. BadRequestを発生させてステータスコード400とエラーコードを返す

   - 必須パラメーターがある場合

     3-2. Paymentクラスのcreateメソッドを実行してPaymentオブジェクトを作成，DBに保存する

     - 登録に成功した場合

       4-1. ステータスコード201と登録したPaymentオブジェクトを返す

     - 登録に失敗した場合

       4-2. BadRequestを発生させて，ステータスコード400とエラーコードを返す

.. _int-sequence-read:

収支を取得する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml::

   autonumber

   actor 利用者
   利用者 -> PaymentsController : read
   PaymentsController -> Payment : find

   autonumber stop
   Payment --> PaymentsController
   PaymentsController --> 利用者

1. リクエストを受けると，PaymentsControllerクラスのreadメソッドを実行する
2. findメソッドでPaymentオブジェクトを取得する
3. ステータスコード200と取得したPaymentオブジェクトを返す

.. _int-sequence-index:

収支を検索する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml::

   autonumber

   actor 利用者
   利用者 -> PaymentsController : index
   PaymentsController -> Payment : where

   autonumber stop
   Payment --> PaymentsController
   PaymentsController --> 利用者

1. リクエストを受けると，PaymentsControllerクラスのindexメソッドを実行する
2. パラメーターからQueryクラスのオブジェクトを作成する
3. valid?メソッドを実行して不正な値がないかチェックする

   - 不正な値がある場合

     4-1. BadRequestを発生させて，ステータスコード400とエラーコードを返す

   - 不正な値がない場合

     4-1. whereメソッドを実行してPaymentオブジェクトの配列を取得する

     4-2. ステータスコード200と取得したPaymentオブジェクトの配列を返す

.. _int-sequence-update:

収支を更新する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml::

   autonumber

   actor 利用者
   利用者 -> PaymentsController : update
   PaymentsController -> Payment : update_attributes

   autonumber stop
   Payment --> PaymentsController
   PaymentsController --> 利用者

1. リクエストを受けると，PaymentsControllerクラスのupdateメソッドを実行する
2. update_attributesメソッドでPaymentオブジェクトを更新する

   - 不正な値がある場合

     3. BadRequestを発生させて，ステータスコード400とエラーコードを返す

   - 不正な値がない場合

     3. ステータスコード200と更新したPaymentオブジェクトを返す

.. _int-sequence-delete:

収支を削除する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml::

   autonumber

   actor 利用者
   利用者 -> PaymentsController : delete
   PaymentsController -> Payment : delete

   autonumber stop
   Payment --> PaymentsController
   PaymentsController --> 利用者

1. リクエストを受けると，PaymentsControllerクラスのdeleteメソッドを実行する
2. Paymentクラスのdeleteメソッドを実行して削除する
3. ステータスコード204を返す

.. _int-sequence-settle:

収支を計算する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml::

   autonumber

   actor 利用者
   利用者 -> PaymentsController : settle
   PaymentsController -> Payment : settle

   autonumber stop
   Payment --> PaymentsController
   PaymentsController --> 利用者

1. リクエストを受けると，PaymentsControllerクラスのsettleメソッドを実行する
2. Paymentクラスのsettleメソッドを実行して収支を計算する
3. パラメーター"interval"をチェックし，その結果に基づいてそれぞれ以下の処理を行う

   - daily or monthly or yearlyの場合

     4-1. intervalに従って収支を計算する

     4-2. ステータスコード200と計算結果を返す

   - それ以外の場合

     4-1. BadRequestを発生させて，ステータスコード400とエラーコードと返す

.. _int-schema:

データベース構成
----------------

データベースは下記のテーブルで構成される

- :ref:`int-schema-users`
- :ref:`int-schema-clients`
- :ref:`int-schema-payments`

.. _int-schema-users:

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

.. _int-schema-clients:

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

.. _int-schema-payments:

payments テーブル
^^^^^^^^^^^^^^^^^

収支を登録するpaymentsテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "PRIMARY KEY", "NOT NULL"

   "id", "INTEGER", "paymentオブジェクトのID", "◯", "◯"
   "payment_type", "STRING", "収入/支出を表すフラグ",, "◯"
   "date", "DATE", "収入/支出があった日",, "◯"
   "content", "STRING", "収入/支出の内容",, "◯"
   "category", "STRING", "収入/支出のカテゴリ",, "◯"
   "price", "INTEGER", "収入/支出の金額",, "◯"
   "created_at", "DATETIME", "収支が登録された日時",, "◯"
   "updated_at", "DATETIME", "収支が登録 or 更新された日時",, "◯"

設計仕様
========

設計仕様では以下を定義する

- `モジュール構成 <http://localhost/algieba_docs/design_spec.html#id2>`__
- `シーケンス <http://localhost/algieba_docs/design_spec.html#id3>`__
- `データベース構成 <http://localhost/algieba_docs/design_spec.html#id10>`__

モジュール構成
--------------

MVCモデルを利用する

*クラス図*

.. uml::

   class Login_View <<Boundary>>
   class Account_View <<Boundary>>

   class LoginController {
     + authenticate_user() ; void
     + authenticate_client() : void
   }

   class AccountsController {
     + manage() : void
     + create(request_parameters : Hash) : void
     + read(id : String) : void
     + index(query_parameters : Hash) : void
     + update(id : String, request_parameters : Hash) : void
     + delete(id : String) : void
     + settle(interval : String) : void
     - account_params() : Array
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

   class Account {
     + account_type : String
     + date : Date
     + content : String
     + category : String
     + price : Fixnum
     + settle(interval : String) : Hash<String, Fixnum>
   }

   class Query {
     - account_type : String
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
   Account_View -right- AccountsController
   AccountsController "1" -- "0..*" Account
   AccountsController -- Query
   AccountsController -- Settlement

- Model

  - User: Usersテーブルを操作するモデル
  - Client: Clientsテーブルを操作するモデル
  - Account: Accountsテーブルを操作するモデル

    - settle: 収支を計算するメソッド

  - Query: 家計簿検索時のクエリを管理するモデル

    - date_valid?: 日付を検証するためのメソッド

  - Settlement: 収支計算時のクエリを管理するモデル

- View

  - Login_View: 認証を行うビュー

    - 利用者が未認証時に表示される

  - Account_View: 家計簿の登録や表示を行うビュー

    - 認証された利用者が家計簿の登録・参照を行う

- Controller

  - LoginController: 認証処理を行うコントローラー

    - authenticate_user: ユーザー認証を行うメソッド
    - authenticate_client: アプリ認証を行うメソッド

  - AccountsController: 家計簿を処理するコントローラ

    - manage: ブラウザに管理画面を表示するメソッド
    - create: 家計簿を登録するメソッド
    - read: 家計簿を取得するメソッド
    - index: 家計簿を検索するメソッド
    - update: 家計簿を更新するメソッド
    - delete: 家計簿を削除するメソッド
    - settle: 収支を計算するメソッド
    - account_params: Accountの属性名の配列を返すメソッド
    - index_params: Queryの属性名の配列を返すメソッド

シーケンス
----------

- `ログインする <http://localhost/algieba_docs/design_spec.html#id4>`__
- `家計簿を登録する <http://localhost/algieba_docs/design_spec.html#id5>`__
- `家計簿を取得する <http://localhost/algieba_docs/design_spec.html#id6>`__
- `家計簿を検索する <http://localhost/algieba_docs/design_spec.html#id7>`__
- `家計簿を更新する <http://localhost/algieba_docs/design_spec.html#id8>`__
- `家計簿を削除する <http://localhost/algieba_docs/design_spec.html#id9>`__
- `収支を計算する <http://localhost/algieba_docs/design_spec.html#id10>`__

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

   autonumber resume
   Login_View -> AccountController
   AccountController -> Account_View

   autonumber stop
   Account_View --> 利用者

1. 利用者がブラウザから本アプリにアクセスする
2. 利用者がユーザーIDとパスワードを入力してログインする
3. LoginControllerがユーザーIDとパスワードが一致するUserオブジェクトを検索する
4. 一致するユーザーが存在しなければLogin_Viewを表示して2へ戻る
5. 一致するユーザーが存在すればAccountController#manageを実行する
6. AccountControllerがAccountを取得してAccount_Viewを表示する

家計簿を登録する
^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml::

   autonumber

   actor 利用者
   利用者 -> AccountsController : create
   AccountsController -> Account : create

   autonumber stop
   Account --> AccountsController
   AccountsController --> 利用者

1. リクエストを受けると，AccountsControllerクラスのcreateメソッドを実行する
2. 必須パラメーターをチェックする

   - 必須パラメーターがない場合

     3-1. BadRequestを発生させてステータスコード400とエラーコードを返す

   - 必須パラメーターがある場合

     3-2. Accountクラスのcreateメソッドを実行してAccountオブジェクトを作成，DBに保存する

     - 登録に成功した場合

       4-1. ステータスコード201と登録したAccountオブジェクトを返す

     - 登録に失敗した場合

       4-2. BadRequestを発生させて，ステータスコード400とエラーコードを返す

家計簿を取得する
^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml::

   autonumber

   actor 利用者
   利用者 -> AccountsController : read
   AccountsController -> Account : find

   autonumber stop
   Account --> AccountsController
   AccountsController --> 利用者

1. リクエストを受けると，AccountsControllerクラスのreadメソッドを実行する
2. findメソッドでAccountオブジェクトを取得する
3. ステータスコード200と取得したAccountオブジェクトを返す

家計簿を検索する
^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml::

   autonumber

   actor 利用者
   利用者 -> AccountsController : index
   AccountsController -> Account : where

   autonumber stop
   Account --> AccountsController
   AccountsController --> 利用者

1. リクエストを受けると，AccountsControllerクラスのindexメソッドを実行する
2. パラメーターからQueryクラスのオブジェクトを作成する
3. valid?メソッドを実行して不正な値がないかチェックする

   - 不正な値がある場合

     4-1. BadRequestを発生させて，ステータスコード400とエラーコードを返す

   - 不正な値がない場合

     4-1. whereメソッドを実行してAccountオブジェクトの配列を取得する

     4-2. ステータスコード200と取得したAccountオブジェクトの配列を返す

家計簿を更新する
^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml::

   autonumber

   actor 利用者
   利用者 -> AccountsController : update
   AccountsController -> Account : update_attributes

   autonumber stop
   Account --> AccountsController
   AccountsController --> 利用者

1. リクエストを受けると，AccountsControllerクラスのupdateメソッドを実行する
2. update_attributesメソッドでAccountオブジェクトを更新する

   - 不正な値がある場合

     3. BadRequestを発生させて，ステータスコード400とエラーコードを返す

   - 不正な値がない場合

     3. ステータスコード200と更新したAccountオブジェクトを返す

家計簿を削除する
^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml::

   autonumber

   actor 利用者
   利用者 -> AccountsController : delete
   AccountsController -> Account : delete

   autonumber stop
   Account --> AccountsController
   AccountsController --> 利用者

1. リクエストを受けると，AccountsControllerクラスのdeleteメソッドを実行する
2. Accountクラスのdeleteメソッドを実行して削除する
3. ステータスコード204を返す

収支を計算する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml::

   autonumber

   actor 利用者
   利用者 -> AccountsController : settle
   AccountsController -> Account : settle

   autonumber stop
   Account --> AccountsController
   AccountsController --> 利用者

1. リクエストを受けると，AccountsControllerクラスのsettleメソッドを実行する
2. Accountクラスのsettleメソッドを実行して収支を計算する
3. パラメーター"interval"をチェックし，その結果に基づいてそれぞれ以下の処理を行う

   - daily or monthly or yearlyの場合

     4-1. intervalに従って収支を計算する

     4-2. ステータスコード200と計算結果を返す

   - それ以外の場合

     4-1. BadRequestを発生させて，ステータスコード400とエラーコードと返す

データベース構成
----------------

データベースは下記のテーブルで構成される

- `users <http://localhost/algieba_docs/design_spec.html#id2>`__
- `clients <http://localhost/algieba_docs/design_spec.html#id2>`__
- `accounts <http://localhost/algieba_docs/design_spec.html#id2>`__

users テーブル
^^^^^^^^^^^^^^

ユーザーを登録するusersテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "主キー", "NOT NULL"

   "id", "INTEGER", "userオブジェクトのID", "◯", "◯"
   "user_id", "STRING", "ユーザーが登録したID",,
   "password", "STRING", "パスワード",,
   "created_at", "DATETIME", "ユーザー情報を作成した日時",,
   "updated_at", "DATETIME", "ユーザー情報を更新した日時",,

clients テーブル
^^^^^^^^^^^^^^^^

アプリを登録するclientsテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "主キー", "NOT NULL"

   "id", "INTEGER", "clientオブジェクトのID", "◯", "◯"
   "application_id", "STRING", "クライアントアプリのID",,
   "application_key", "STRING", "クライアントアプリのキー",,
   "created_at", "DATETIME", "アプリ情報を作成した日時",,
   "updated_at", "DATETIME", "アプリ情報を更新した日時",,

accounts テーブル
^^^^^^^^^^^^^^^^^

家計簿を登録するaccountsテーブルを定義する

.. csv-table::
   :header: "カラム", "型", "内容", "主キー", "NOT NULL"

   "id", "INTEGER", "家計簿のID", "◯", "◯"
   "account_type", "STRING", "収入/支出を表すフラグ",, "◯"
   "date", "DATE", "収入/支出があった日",, "◯"
   "content", "STRING", "収入/支出の内容",, "◯"
   "category", "STRING", "収入/支出のカテゴリ",, "◯"
   "price", "INTEGER", "収入/支出の金額",, "◯"
   "created_at", "DATETIME", "家計簿が登録された日時",, "◯"
   "updated_at", "DATETIME", "家計簿が登録or更新された日時",, "◯"

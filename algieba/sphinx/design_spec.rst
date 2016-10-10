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

   class Account_View <<Boundary>>

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

   class Account {
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

   Account_View -right- AccountsController
   AccountsController "1" -- "0..*" Account
   AccountsController -- Query
   AccountsController -- Settlement

- Model

  - Account: Accountsテーブルを操作するモデル

    - settle: 収支を計算するメソッド

  - Query: 家計簿検索時のクエリを管理するモデル

    - date_valid?: 日付を検証するためのメソッド

  - Settlement: 収支計算時のクエリを管理するモデル

- View

  - Account_View: 家計簿の登録や表示を行うビュー

    - 利用者がhttp://<ホスト名>(:80)/にアクセスすることで表示される

- Controller

  - AccountsController: リクエストを処理するコントローラ

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

- `家計簿を登録する <http://localhost/algieba_docs/design_spec.html#id4>`__
- `家計簿を取得する <http://localhost/algieba_docs/design_spec.html#id5>`__
- `家計簿を検索する <http://localhost/algieba_docs/design_spec.html#id6>`__
- `家計簿を更新する <http://localhost/algieba_docs/design_spec.html#id7>`__
- `家計簿を削除する <http://localhost/algieba_docs/design_spec.html#id8>`__
- `収支を計算する <http://localhost/algieba_docs/design_spec.html#id9>`__

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

家計簿を登録するAccountテーブルを定義する

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

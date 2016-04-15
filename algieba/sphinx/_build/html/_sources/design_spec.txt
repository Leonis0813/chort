設計仕様
========

設計仕様では以下を定義する

- `モジュール構成 <http://localhost/algieba_docs/design_spec.html#id2>`__
- `処理手順 <http://localhost/algieba_docs/design_spec.html#id3>`__
- `データベース構成 <http://localhost/algieba_docs/design_spec.html#id9>`__
- `Web API <http://localhost/algieba_docs/design_spec.html#web-api>`__

モジュール構成
--------------

MVCモデルを利用する

*クラス図*

.. image:: images/class.jpg
   :alt: クラス図

- Model

  - Account: Accountsテーブルを操作するモデル

    - show: レコードを取得するメソッド
    - update: レコードを更新するメソッド
    - destroy: レコードを削除するメソッド
    - settle: 収支を計算するメソッド
    - check_condition: 家計簿の検索条件をチェックするメソッド

- View

  - Account_View: 家計簿の登録や表示を行うビュー

    - 利用者がhttp://<ホスト名>/accountsにアクセスすることで表示される

- Controller

  - Accounts_Controller: リクエストを処理するコントローラ

    - create: 家計簿を登録するメソッド
    - read: 家計簿を検索するメソッド
    - update: 家計簿を更新するメソッド
    - delete: 家計簿を削除するメソッド
    - settle: 収支を計算するメソッド

処理手順
--------

- `家計簿を登録する <http://localhost/algieba_docs/design_spec.html#id4>`__
- `家計簿を取得する <http://localhost/algieba_docs/design_spec.html#id5>`__
- `家計簿を更新する <http://localhost/algieba_docs/design_spec.html#id6>`__
- `家計簿を削除する <http://localhost/algieba_docs/design_spec.html#id7>`__
- `収支を見る <http://localhost/algieba_docs/design_spec.html#id8>`__

家計簿を登録する
^^^^^^^^^^^^^^^^

.. image:: images/seq_create.jpg

1. Viewer, Registerからのリクエストを受信すると，Accounts_Controllerクラスのcreateメソッドを実行する
2. Accountクラスのcreateメソッドを実行して家計簿を登録する
3. createメソッドの実行結果に基づいてそれぞれ以下の処理を行う

   - Accountクラスのインスタンスを取得した場合

     3-1. Viewer, Registerにステータスコード201を送信する

   - 例外が発生した場合

     3-1. Viewer, Registerにエラーコードとステータスコード400を送信する

家計簿を検索する
^^^^^^^^^^^^^^^^

.. image:: images/seq_read.jpg

1. Viewerからのリクエストを受信すると，Accounts_Controllerクラスのreadメソッドを実行する
2. Accountクラスのshowメソッドをパラメータを引数にして実行する
3. check_conditionを実行し，その結果に基づいてそれぞれ以下の処理を行う

   - 空配列の場合

     3-1. whereメソッドを実行して家計簿を取得する

     3-2. Viewerに検索結果とステータスコード200を送信する

   - 空配列でない場合（不正なパラメータがある場合）

     3-1. Viewerにエラーコードとステータスコード400を送信する

家計簿を更新する
^^^^^^^^^^^^^^^^

.. image:: images/seq_update.jpg

1. Viewerからのリクエストを受信すると，Accounts_Controllerクラスのupdateメソッドを実行する
2. Accountクラスのupdateメソッドをパラメータを引数にして実行する
3. check_conditionを実行し，その結果に基づいてそれぞれ以下の処理を行う

   - 空配列の場合

     3-1. whereメソッドを実行して家計簿を取得する

     3-2. 取得した家計簿それぞれに対して，updateメソッドを実行して家計簿を更新する

     3-3. Viewerに更新結果とステータスコード200を送信する

   - 空配列でない場合（不正なパラメータがある場合）

     3-1. Viewerにエラーコードとステータスコード400を送信する

家計簿を削除する
^^^^^^^^^^^^^^^^

.. image:: images/seq_delete.jpg

1. Viewerからのリクエストを受信すると，Accounts_Controllerクラスのdeleteメソッドを実行する
2. Accountクラスのdestroyメソッドをパラメータを引数にして実行する
3. check_conditionを実行し，その結果に基づいてそれぞれ以下の処理を行う

   - 空配列の場合

     3-1. whereメソッドを実行して家計簿を取得する

     3-2. 取得した家計簿それぞれに対して，deleteメソッドを実行して家計簿を削除する

     3-3. Viewerにステータスコード204を送信する

   - 空配列でない場合（不正なパラメータがある場合）

     3-1. Viewerにエラーコードとステータスコード400を送信する

収支を見る
^^^^^^^^^^

.. image:: images/seq_settle.jpg

1. Viewerからのリクエストを受信すると，Accounts_Controllerクラスのsettleメソッドが実行される
2. パラメータ"interval"をチェックし，その結果に基づいてそれぞれ以下の処理を行う

   - daily or monthly or yearlyの場合

     3-1. intervalに従って収支を計算する

     3-2. Viewerに計算結果とステータスコード200を送信する

   - それ以外の場合

     3-1. Viewerにエラーコードとステータスコード400を送信する

データベース構成
----------------

家計簿情報を登録するAccountテーブルを定義する

- Accountテーブル

+---------------+----------+-----------------------+----------+------------+
| カラム        | 型       | 内容                  | 主キー   | NOT NULL   |
+===============+==========+=======================+==========+============+
| account_type  | STRING   | 収入/支出を表すフラグ |          | ◯          |
+---------------+----------+-----------------------+----------+------------+
| date          | DATE     | 収入/支出があった日   |          | ◯          |
+---------------+----------+-----------------------+----------+------------+
| content       | STRING   | 収入/支出の内容       |          | ◯          |
+---------------+----------+-----------------------+----------+------------+
| category      | STRING   | 収入/支出のカテゴリ   |          | ◯          |
+---------------+----------+-----------------------+----------+------------+
| price         | INTEGER  | 収入/支出の金額       |          | ◯          |
+---------------+----------+-----------------------+----------+------------+

Web API
-------

共通定義
^^^^^^^^

入力項目
""""""""

  +-------------+------------------------------+
  |項目名       |フォーマット                  |
  +=============+==============================+
  | account_type| "income" または "expense"    |
  +-------------+------------------------------+
  |         date| yyyy-mm-dd                   |
  +-------------+------------------------------+
  |      content| 任意の英数字・日本語         |
  +-------------+------------------------------+
  |     category| 任意の英数字・日本語         |
  +-------------+------------------------------+
  |        price| 0以上の整数                  |
  +-------------+------------------------------+

  - *項目の値が空ハッシュや空配列の場合はその項目はないものとして見なす*

エラーコード
""""""""""""

  +----------------------------+----------------------------+
  |エラーコード                |意味                        |
  +============================+============================+
  |absent_param_[項目名]       |入力必須の項目がない        |
  +----------------------------+----------------------------+
  |invalid_param_[項目名]      |不正値のパラメータがある    |
  +----------------------------+----------------------------+

API
^^^^

以下のAPIを定義する

- `家計簿を登録する <http://localhost/algieba_docs/design_spec.html#id13>`__
- `家計簿を検索する <http://localhost/algieba_docs/design_spec.html#id14>`__
- `家計簿を更新する <http://localhost/algieba_docs/design_spec.html#id15>`__
- `家計簿を削除する <http://localhost/algieba_docs/design_spec.html#id16>`__
- `収支を見る <http://localhost/algieba_docs/design_spec.html#id17>`__

家計簿を登録する
""""""""""""""""

HTTP Method： POST

Path：/accounts

Request Body：
  - 必須

    - accounts

      - account_type
      - date
      - content
      - category
      - price

Response：
  - 登録成功時

    Status Code： 201

    Body： 登録した家計簿

  - 登録失敗時

    Status Code： 400

    Body： `エラーコード <http://localhost/algieba_docs/design_spec.html#id12>`__

家計簿を検索する
""""""""""""""""

HTTP Method： GET

Path：/accounts

Query：

  クエリがない場合は全ての家計簿を取得する

  - account_type
  - date
  - content
  - category
  - price

Request Body：なし

Response：
  - 検索成功時

    Status Code： 200
	  
    Body： 取得した家計簿の配列

  - 検索失敗時

    Status Code： 400

    Body： `エラーコード <http://localhost/algieba_docs/design_spec.html#id12>`__

家計簿を更新する
""""""""""""""""

HTTP Method： PUT

Path：/accounts

Request Body：
  - 必須

    - with

      - account_type
      - date
      - content
      - category
      - price

  - オプション（指定がなければ全ての家計簿が更新される）

    - condition

      - account_type
      - date
      - content
      - category
      - price

Response：
  - 更新成功時

    Status Code： 200

    Body： 更新されたレコードの配列

  - 更新失敗時

    Status Code： 400

    Body： `エラーコード <http://localhost/algieba_docs/design_spec.html#id12>`__

家計簿を削除する
""""""""""""""""

HTTP Method： DELETE

Path：/accounts

Request Body：

  指定がない場合は全ての家計簿を削除する

  - condition

    - account_type
    - date
    - content
    - category
    - price

Response ：
  - 削除成功時

    Status Code： 204

    Body： なし

  - 削除失敗時

    Status Code： 400

    Body： `エラーコード <http://localhost/algieba_docs/design_spec.html#id12>`__

収支を見る
""""""""""

HTTP Method： GET

Path： /settlement

Query：

  - 必須

    - interval

      - yearly, monthly, dailyのどれか

Request Body： なし

Response：
  - 収支計算成功時

    Status Code： 200

    Body： 収支のリスト

  - 収支計算失敗時

    Status Code： 400

    Body： `エラーコード <http://localhost/algieba_docs/design_spec.html#id12>`__

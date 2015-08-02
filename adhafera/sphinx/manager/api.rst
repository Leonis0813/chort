API仕様
=======

共通定義
--------

入力項目
^^^^^^^^

  +---------+--------------------+
  |項目名   |フォーマット        |
  +=========+====================+
  |     date|yyyy-mm-dd          |
  +---------+--------------------+
  |  content|任意の英数字・日本語|
  +---------+--------------------+
  | category|任意の英数字・日本語|
  +---------+--------------------+
  |    price|0以上の整数         |
  +---------+--------------------+

エラーコード
^^^^^^^^^^^^

  +----------------------------+------------------------+
  |エラーコード                |意味                    |
  +============================+========================+
  |absent_param_[項目名]       |入力必須の項目がない    |
  +----------------------------+------------------------+
  |invalid_value_[項目名]      |入力項目の値が不正      |
  +----------------------------+------------------------+
  |invalid_param_[パラメータ名]|不正なパラメータがある  |
  +----------------------------+------------------------+

API
----

家計簿を登録する
^^^^^^^^^^^^^^^^

HTTP Method： POST

Path：/[:table]

Request Body：
	- accounts

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

	  Body： エラーコード

家計簿を検索する
^^^^^^^^^^^^^^^^

HTTP Method： GET

Path：/[:table]

Query：
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

	  Body： エラーコード

家計簿を更新する
^^^^^^^^^^^^^^^^

HTTP Method： PUT

Path：/[:table]

Request Body：
	- condition

	  - date
	  - content
	  - category
	  - price

	- with

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

	  Body： エラーコード

家計簿を削除する
^^^^^^^^^^^^^^^^

HTTP Method： DELETE

Path：/[:table]/[:id]

Request Body：なし

Response ：
	 - 削除成功時

	   Status Code： 204

	   Body： なし

	 - 削除失敗時

	   Status Code： 400

	   Body： エラーコード

収支を見る
^^^^^^^^^^

HTTP Method： GET

Path： /settlement

Query：
	- period

	  - monthly, weekly, dailyのどれか

Request Body： なし

Response：
	- 収支計算成功時

	  Status Code： 200

	  Body： 収支のリスト

	- 収支計算失敗時

	  Status Code： 400

	  Body： エラーコード

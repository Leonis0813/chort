詳細仕様
============

- システムの詳細な振る舞いと構造を記載する

  - `振る舞い <http://localhost/regulus_docs/internal.html#id2>`__
  - `構造 <http://localhost/regulus_docs/internal.html#id5>`__

振る舞い
--------

  - `通貨の価格変動を確認する <http://localhost/regulus_docs/internal.html#id3>`__
  - `変動に関連する情報を取得する <http://localhost/regulus_docs/internal.html#id4>`__

通貨の価格変動を確認する
^^^^^^^^^^^^^^^^^^^^^^^^

**シーケンス図**

.. image:: images/seq_graph_int.jpg
   :alt: シーケンス図(通貨の価格変動を確認する)

- 利用者がWebページにアクセスしてからグラフを確認するまでの流れ

  1. アクセスを受けたConfirmation\_viewがConfirmation\_Controllerにアクセスの受信を通知する
  2. 受信したConfirmation\_Controllerがビューを表示する（この時点では何も表示されない）
  3. Currencies\_ControllerがCurrencyオブジェクトの配列を取得して返す
  4. ビューにグラフを表示する
  5. 以降は定期的に通貨情報の取得を繰り返す

変動に関連する情報を取得する
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**シーケンス図**

.. image:: images/seq_info_int.jpg
   :alt: シーケンス図(変動に関連する情報を取得する)

- 利用者がWebページにアクセスしてから関連情報を確認するまでの流れ

  1. アクセスを受けたConfirmation\_viewがConfirmation\_Controllerにアクセスの受信を通知する
  2. 受信したConfirmation\_Controllerがビューを表示する（この時点では何も表示されない）
  3. Tweets\_ControllerがTweetオブジェクトを取得して返す
  4. Articles\_ControllerがArticleオブジェクトを取得して返す
  5. ビューに情報を表示する
  6. 以降は定期的にツイートと記事の取得を繰り返す

構造
----

**クラス図**

.. image:: images/class_int.jpg
   :alt: クラス図

- MVCモデルを利用する

- View

  - confirmation\_view

    - Webブラウザ上で表示する画面

- Controller

  - Confirmation\_Controller

    - confirmation\_viewのコントローラ
    - グラフや関連情報の更新を行う

  - Currencies\_Controller

    - Currencyクラスのコントローラ
    - Currencyオブジェクトを取得し，ビューに表示する

  - Tweets\_Controller

    - Tweetクラスのコントローラ
    - Tweetオブジェクトを取得し，ビューに表示する

  - Articles\_Controller

    - Articleクラスのコントローラ
    - Articleオブジェクトを取得し，ビューに表示する

- Model

  - Currency

    - 通貨情報を表すクラス
    - 以下の情報を保持する

      - from_date: 集計対象データの範囲（開始時刻）
      - to_date: 集計対象データの範囲（終了時刻）
      - pair: 為替のペペアコード（例：USDJPY）
      - interval: 集計対象期間

	- 以下のいずれかが保持されている

	  - 5-min
	  - 10-min
	  - 20-min
	  - 30-min
	  - 1-hour
	  - 2-hour
	  - 3-hour
	  - 6-hour
	  - 12-hour
	  - 1-day
	  - 1-week
	  - 1-month
	  - 2-month
	  - 3-month
	  - 6-month
	  - 1-year

      - open: 始値
      - close: 終値
      - high: 高値
      - low: 安値
      - created_at: 通貨情報が作成された日時
      - updated_at: 通貨情報が更新された日時

  - Tweet

    - ツイートを表すクラス
    - 以下の情報を保持する

      - tweet_id: ツイートのID
      - user_name: ツイートのユーザー名
      - profile_image_url: アカウントのプロフィール画像のURL
      - full_text: ツイート本文
      - tweeted_at: ツイート日時
      - created_at: ツイート情報が作成された日時

  - Article

    - 記事を表すクラス
    - 以下の情報を保持する

      - published: 記事が発行された日時
      - title: 記事のタイトル
      - summary: 記事の要約
      - url: 記事へのURL
      - created_at: 記事情報が作成された日時

  - **データベースには外部スクリプトにより定期的にレコードが追加される**

    - **通貨，ツイート・日経の情報を取得するスクリプトが定期的に実行されてMySQLに登録される**

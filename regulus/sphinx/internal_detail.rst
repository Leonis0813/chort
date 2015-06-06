内部詳細仕様
============

-  システムの詳細な振る舞いと構造を記載する

   -  `振る舞い <http://localhost:8888/regulus_docs/internal_detail.html#id2>`__
   -  `構造 <http://localhost:8888/regulus_docs/internal_detail.html>`__

振る舞い
--------

通貨の価格変動を確認する
^^^^^^^^^^^^^^^^^^^^^^^^

シーケンス図
            

.. figure:: http://localhost:8888/regulus_docs/_images/sequence_graph_detail.jpg
   :alt: シーケンス図(通貨の価格変動を確認する)

-  利用者がWebページにアクセスしてからグラフを確認するまでの流れ

   1. アクセスを受けたConfirmation\_viewがConfirmation\_Controllerにアクセスの受信を通知する
   2. 受信したConfirmation\_Controllerが通貨取得を開始する
   3. WebAPIを利用して外部から通貨情報を取得する
   4. 取得した情報をパースする
   5. Currencyオブジェクトを作成して返す
   6. Currencyオブジェクトから必要な情報を取得してグラフを更新する

-  以降は3〜6を繰り返す

変動に関連する情報を取得する
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

シーケンス図
            

.. figure:: http://localhost:8888/regulus_docs/_images/sequence_info_detail.jpg
   :alt: シーケンス図(変動に関連する情報を取得する)

-  利用者がWebページにアクセスしてから関連情報を確認するまでの流れ

   1. アクセスを受けたConfirmation\_viewがConfirmation\_Controllerにアクセスの受信を通知する
   2. 受信したConfirmation\_Controllerが関連情報の取得を開始する
   3. WebAPIを利用してTwitterからツイートを取得する
   4. 取得したツイートをパースする
   5. Tweetオブジェクトを作成して返す
   6. WebAPIを利用して日経から記事を取得する
   7. 取得した記事をパースする
   8. Articleオブジェクトを作成して返す
   9. ブラウザを更新する

-  以降は3〜9を繰り返す

構造
----

クラス図
        


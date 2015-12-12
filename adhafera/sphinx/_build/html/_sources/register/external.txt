機能仕様
========

- 本モジュールの振る舞いと構造を記載する

  - `振る舞い <http://localhost/adhafera_docs/register/external.html#id2>`__
  - `構造 <http://localhost/adhafera_docs/register/external.html#id4>`__

振る舞い
--------

- `家計簿を登録する <http://localhost/adhafera_docs/register/external.html#id3>`__

家計簿を登録する
^^^^^^^^^^^^^^^^

**シーケンス図**

.. image:: images/seq_register_ext.jpg
   :alt: シーケンス図(家計簿を登録する)

- 利用者が登録画面を開いてから家計簿を登録するまでの流れ

  1. 家計簿情報を入力して登録ボタンを押す
  2. 画面管理モジュールが情報を受け取る
  3. 入力情報が十分かをチェックする
  4. 日付情報のフォーマットが正しいかをチェックする

     - 日付は'yyyy-mm-dd'のフォーマットのみ受け付ける

  5. 金額が正しいかをチェックする

     - 金額は正の整数のみ受け付ける

  6. 入力情報をデータベースのあるサーバに送信する
  7. 送信した結果を登録画面に表示する

構造
----

**クラス図**

.. image:: images/class_ext.jpg
   :alt: クラス図

- MVCモデルを利用する

- Model

  - データベースに登録するため本モジュールにはモデルは存在しない

- View

  - 登録画面
    
    - 家計簿情報の入力を受け付ける

- Controller

  - 画面管理

    - 画面の表示に関する処理を管理するコントローラ

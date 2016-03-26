詳細仕様
========

- システムの詳細な振る舞いと構造を記載する

  - `振る舞い <http://localhost/zosma_docs/register/internal.html#id2>`__
  - `構造 <http://localhost/zosma_docs/register/internal.html#id4>`__

振る舞い
--------

- `家計簿を検索する <http://localhost/zosma_docs/register/internal.html#id3>`__

家計簿を検索する
^^^^^^^^^^^^^^^^

**シーケンス図**

.. image:: images/seq_index_int.jpg
   :alt: シーケンス図(家計簿を検索する)

- 利用者が検索画面を開いてから家計簿を表示するまでの流れ

  1. 利用者が検索条件を入力して登録ボタンを押すと，actionPerformedメソッドが実行される
  2. 入力された条件を引数にして，searchAccountsメソッドが実行される
  3. checkDateメソッドで日付のフォーマットのチェックを行う
  4. checkPriceメソッドで金額のチェックを行う
  5. 不正な条件があればMessageFieldにエラーを通知する
  6. 入力された条件に問題が無ければ，getAccountsメソッドを実行してリクエストを送信する
  7. 取得した家計簿をテーブルに表示する

構造
----

**クラス図**

.. image:: images/class_int.jpg
   :alt: クラス図

- MVCモデルを利用する

- Model

  - データベースに登録されている情報を表示するだけのモジュールであるためModelは存在しない

- View

  - AccountTable

    - 取得した家計簿を表示するテーブル

  - SerachForm

    - 検索条件を入力するテキストフィールド

  - MessageField

    - エラー等を表示するテキストフィールド

- Controller

  - ApplicationController

    - アプリ全体を管理するコントローラー

  - ViewController

    - ビューの各コンポーネントを管理するコントローラー

- InputChecker

  - 入力された条件のフォーマットが正しいかどうかをチェックするクラス

- HTTPClient

  - DBサーバにリクエストを送信するクラス

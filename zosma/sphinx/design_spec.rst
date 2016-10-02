詳細仕様
========

設計仕様では以下を定義する

  - `モジュール構成 <http://localhost/zosma_docs/design_spec.html#id2>`__
  - `処理手順 <http://localhost/zosma_docs/design_spec.html#id3>`__

モジュール構成
--------------

**クラス図**

.. image:: images/class.jpg
   :alt: クラス図
   :align: center

- MVCモデルを利用する

  - Model

    - データベースに登録されている情報を表示するだけのモジュールであるためModelは存在しない

  - View

    - AccountTable

      - 取得した家計簿を表示するテーブル

    - SerachForm

      - 検索フォームを表すクラス

        - DateForm

          - 日付に関する条件を入力するフォーム
          - dateBefore: 指定した日付以前の家計簿を検索する
          - dateAfter: 指定した日付以降の家計簿を検索する
          - dateBefore, dateAfterとも指定した日付は検索対象に含まれる

        - ContentForm

          - 内容に関する条件を入力するフォーム
          - content: 指定した内容の家計簿を検索する
          - contentType: 全文一致か部分一致かを指定する

        - CategoryForm

          - カテゴリに関する条件を入力するフォーム
          - category: 指定したカテゴリに一致する家計簿を検索する

        - PriceForm

          - 金額に関する条件を入力するフォーム
          - priceUpper: 指定した金額以上の家計簿を検索する
          - priceLower: 指定した金額以下の家計簿を検索する
          - priceUpper, priceLowerとも指定した金額は検索対象に含まれる

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

処理手順
--------

- `家計簿を検索する <http://localhost/zosma_docs/design_spec.html#id4>`__

家計簿を検索する
^^^^^^^^^^^^^^^^

**シーケンス図**

.. image:: images/seq_index.jpg
   :alt: シーケンス図(家計簿を検索する)
   :align: center

- 利用者が検索画面を開いてから家計簿を表示するまでの流れ

  1. 利用者が検索条件を入力して登録ボタンを押すと，actionPerformedメソッドが実行される
  2. 入力された条件を引数にして，searchAccountsメソッドが実行される
  3. checkDateメソッドでDateFormのdateBeforeに格納されている値のフォーマットのチェックを行う
  4. checkDateメソッドでDateFormのdateAfterに格納されている値のフォーマットのチェックを行う
  5. checkPriceメソッドでPriceFormのpriceUpperに格納されている値のチェックを行う
  6. checkPriceメソッドでPriceFormのpriceLowerに格納されている値のチェックを行う
  7. 不正な条件があればMessageFieldにエラーを通知する文字列をセットする
  8. さらに，SearchForm内の不正な入力があった項目に対応するラベルにチェックマークをセットする
  9. 入力された条件に問題が無ければ，getAccountsメソッドを実行してリクエストを送信する
  10. 取得した家計簿をテーブルに表示する
  11. 取得した家計簿の数をMessageFieldに表示する

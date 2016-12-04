詳細仕様
========

設計仕様では以下を定義する

- :ref:`zos-int-class`
- :ref:`zos-int-sequence`

.. _zos-int-class:

モジュール構成
--------------

*クラス図*

.. uml:: umls/class.uml

- MVCモデルを利用する

  - Model

    - データベースに登録されている情報を表示するだけのモジュールであるためModelは存在しない

  - View

    - PaymentTable

      - 取得した収支を表示するテーブル

    - SerachForm

      - 検索フォームを表すクラス

        - DateForm

          - 日付に関する条件を入力するフォーム
          - dateBefore: 指定した日付以前の収支を検索する
          - dateAfter: 指定した日付以降の収支を検索する
          - dateBefore, dateAfterとも指定した日付は検索対象に含まれる

        - ContentForm

          - 内容に関する条件を入力するフォーム
          - content: 指定した内容の収支を検索する
          - contentType: 全文一致か部分一致かを指定する

        - CategoryForm

          - カテゴリに関する条件を入力するフォーム
          - category: 指定したカテゴリに一致する収支を検索する

        - PriceForm

          - 金額に関する条件を入力するフォーム
          - priceUpper: 指定した金額以上の収支を検索する
          - priceLower: 指定した金額以下の収支を検索する
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

.. _zos-int-sequence:

処理手順
--------

- :ref:`zos-int-sequence-index`

.. _zos-int-sequence-index:

収支を検索する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/sequece-index.uml

- 利用者が検索画面を開いてから収支を表示するまでの流れ

  1. 利用者が検索条件を入力して登録ボタンを押すと，actionPerformedメソッドが実行される
  2. 入力された条件を引数にして，searchPaymentsメソッドが実行される
  3. checkDateメソッドでDateFormのdateBeforeに格納されている値のフォーマットのチェックを行う
  4. checkDateメソッドでDateFormのdateAfterに格納されている値のフォーマットのチェックを行う
  5. checkPriceメソッドでPriceFormのpriceUpperに格納されている値のチェックを行う
  6. checkPriceメソッドでPriceFormのpriceLowerに格納されている値のチェックを行う
  7. 不正な条件があればMessageFieldにエラーを通知する文字列をセットする
  8. さらに，SearchForm内の不正な入力があった項目に対応するラベルにチェックマークをセットする
  9. 入力された条件に問題が無ければ，getPaymentsメソッドを実行してリクエストを送信する
  10. 取得した収支をテーブルに表示する
  11. 取得した収支の数をMessageFieldに表示する

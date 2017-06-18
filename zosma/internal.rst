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

            - CategoryDialog

              - 検索したいカテゴリを選択するためのダイアログ
              - categories: カテゴリの一覧

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

シーケンス
----------

- :ref:`zos-int-sequence-index`

.. _zos-int-sequence-index:

収支を検索する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/sequence-index.uml

- 利用者が検索画面を開いてから収支を表示するまでの流れ

  1. 利用者がアプリを起動すると，ApplicationControllerが作成される
  2. ApplicationControllerはViewControllerを作成する
  3. ViewControllerはSearchFormを作成する
  4. SearchFormはCategoryFormを作成する
  5. ViewControllerはPaymentTableを作成する
  6. ViewControllerはMessageFieldを作成する
  7. ViewControllerはInputCheckerを作成する
  8. 利用者がカテゴリ選択ボタンを押下した場合，14までの処理が実行される
  9. CategoryFormはApplicationControllerのgetCategoryメソッドを実行してカテゴリ一覧を取得する
  10. ApplicationControllerはHTTPClientを作成する
  11. ApplicationControllerはHTTPClientクラスのgetCategoriesメソッドを実行してサーバーからカテゴリ情報を取得する
  12. CategoryFormはCategoryDialogを作成し，取得したカテゴリ名を表示する
  13. 利用者はカテゴリを選択し，OKもしくはキャンセルボタンを押下する
  14. CategoryDialogはCategoryFormクラスのsetCategoryメソッドを実行して選択されたカテゴリ名をテキストフィールドに表示する
  15. 利用者が検索条件を入力して登録ボタンを押すと，SearchFormクラスのactionPerformedメソッドが実行される
  16. SearchFormはApplicationControllerクラスのsearchPaymentsメソッドを実行して収支情報を検索する
  17. InputCheckerクラスのcheckDateメソッドでDateFormのdateBeforeに格納されている値のフォーマットのチェックを行う
  18. InputCheckerクラスのcheckDateメソッドでDateFormのdateAfterに格納されている値のフォーマットのチェックを行う
  19. InputCheckerクラスのcheckPriceメソッドでPriceFormのpriceUpperに格納されている値のチェックを行う
  20. InputCheckerクラスのcheckPriceメソッドでPriceFormのpriceLowerに格納されている値のチェックを行う
  21. 不正な条件があればViewControllerのnoticeInputErrorメソッドを実行してMessageFieldにエラーを通知する文字列をセットする
  22. ViewControllerはMessageFieldクラスのshowMessageメソッドを実行してエラーメッセージを表示する
  23. さらに，SearchFormクラスのshowWrongInputメソッドを実行して不正な入力があった項目に対応するラベルにチェックマークをセットする
  24. 入力された条件に問題が無ければ，ViewControllerクラスのresetMessageFieldを実行してメッセージを初期化する
  25. ViewControllerはMessageFieldクラスのshowMessageメソッドを実行して空文字を表示する
  26. ApplicationControllerはHTTPClientクラスのgetPaymentsメソッドを実行してサーバーから検索条件を満たす収支情報を取得する
  27. ApplicationControllerはViewControllerクラスのresetTablesメソッドを実行してテーブルに表示されている収支情報を削除する
  28. ApplicationControllerはViewControllerクラスのshowPaymentsメソッドを実行してテーブルに取得した収支情報を表示する
  29. ViewControllerはPaymentTableクラスのaddPaymentメソッドを実行して収支情報をテーブルに追加する
  30. ViewControllerはMessageFieldクラスのshowMessageメソッドを実行して取得した収支情報の件数を表示する

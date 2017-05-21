設計仕様
========

設計仕様では以下を定義する

- :ref:`adh-int-class`
- :ref:`adh-int-sequence`

.. _adh-int-class:

モジュール構成
--------------

*クラス図*


.. uml:: umls/class.uml

- MVCモデルを利用する

- Model

  - データベースに登録するため本モジュールにはモデルは存在しない

- View

  - RegistrationView

    - MainActivityから渡される情報を表示するクラス
    - 下記のビューを含む

      - InputView

        - 入力項目を表す抽象クラス
        - 下記の４つのビューを抽象化している

          - DateView

            - 日付に関する情報を表すクラス

          - ContentView

            - 内容に関する情報を表すクラス

          - CategoryView

            - カテゴリに関する情報を表すクラス

          - PriceView

            - 金額に関する情報を表すクラス

- Controller

  - MainActivity

    - 画面からの情報やデータベースから取得したデータを処理するクラス

- InputChecker

  - 正しく入力されているかをチェックするクラス
  - 全項目が入力されているかをチェックする
  - 日付，金額が正しく入力されているかをチェックする

- HTTPClient

  - データベースサーバへアクセスし，以下を行うクラス

    - 収支情報の登録
    - 収支の取得
    - カテゴリ情報の取得

.. _adh-int-sequence:

シーケンス
----------

- `収支を登録する <http://localhost/adhafera_docs/design_spec.html#id4>`__

収支を登録する
^^^^^^^^^^^^^^

.. uml:: umls/sequence-create.uml

1. 利用者がアプリを起動するとonCreateメソッドが実行される
2. settleメソッドを実行して収支を取得する
3. 収支を取得するためにHTTPClientクラスのインスタンスを作成する
4. credentialメソッドを実行してWebAPIを利用するためのAuthorizationヘッダーをセットする
5. sendRequestメソッドを実行してWebAPIを実行し，収支を取得する
6. showSettlementメソッドを実行して今月の収支を表示する
7. getCategoriesメソッドを実行してカテゴリ情報を取得する
8. カテゴリ情報を取得するためにHTTPClientクラスのインスタンスを作成する
9. credentialメソッドを実行してWebAPIを利用するためのAuthorizationヘッダーをセットする
10. sendRequestメソッドを実行してWebAPIを実行し，カテゴリ情報を取得する
11. setCategoriesメソッドを実行してCategoryViewにカテゴリ情報をセットする
12. 利用者が登録ボタンを押下するとonClickメソッドが実行される
13. registPaymentメソッドを実行して入力情報を登録する
14. checkEmptyメソッドを実行して入力が空の項目がないかチェックする
15. 空の項目がある場合はshowMessageメソッドを実行してエラーメッセージを表示する
16. さらにshowWrontInputメソッドを実行して不正な入力項目にチェックマークを付ける
17. 空の項目がない場合は，checkDateメソッドを実行して入力された日付のフォーマットをチェックする
18. 日付が不正な場合は，showMessageメソッドを実行してエラーメッセージを表示する
19. さらにshowWrontInputメソッドを実行して日付入力項目にチェックマークを付ける
20. 日付のフォーマットが正しい場合は，checkPriceメソッドを実行して入力された金額をチェックする
21. 金額が不正な場合は，showMessageメソッドを実行してエラーメッセージを表示する
22. さらにshowWrontInputメソッドを実行して金額入力項目にチェックマークを付ける
23. 金額が正しい場合は，収支情報を登録するためのHTTPClientクラスのインスタンスを作成する
24. credentialメソッドを実行してWebAPIを利用するためのAuthorizationヘッダーをセットする
25. sendRequestメソッドを実行してWebAPIを実行し，収支情報を登録する
26. showMessageメソッドを実行して収支情報が登録された旨を通知する
27. resetFieldメソッドを実行して入力フォームを空文字にする
28. setTodayメソッドを実行して日付入力欄にアプリ起動時の日付をセットする
29. settleメソッドを実行して収支を取得する
30. 収支を取得するためにHTTPClientクラスのインスタンスを作成する
31. credentialメソッドを実行してWebAPIを利用するためのAuthorizationヘッダーをセットする
32. sendRequestメソッドを実行してWebAPIを実行し，収支を取得する
33. showSettlementメソッドを実行して今月の収支を表示する

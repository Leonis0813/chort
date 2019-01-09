設計仕様
========

設計仕様では以下を定義する

- :ref:`adh-int-class`
- :ref:`adh-int-seq`

.. _adh-int-class:

モジュール構成
--------------

*クラス図*


.. uml:: umls/class.uml

- MVCモデルを利用する

- Model

  - データベースに登録するため本モジュールにはモデルは存在しない

- View

  - CreateView

    - MainActivityから渡される情報を表示するクラス
    - 下記のビューを含む

      - InputView

        - 入力項目を表す抽象クラス
        - 下記の４つのビューを抽象化している

          - DateView

            - 日付に関する情報を表示するビュー

          - ContentView

            - 内容に関する情報を表示するビュー

          - CategoryView

            - カテゴリに関する情報を表示するビュー

          - PriceView

            - 金額に関する情報を表示するビュー

  - IndexView

    - IndexActivityから渡される情報を表示するクラス
    - 下記のビューを含む

      - PeriodView

        - 期間に関する情報を表示するビュー

      - ContentView

        - 内容に関する情報を表示するビュー

      - CategoryView

        - カテゴリに関する情報を表示するビュー

      - PriceView

        - 金額に関する情報を表示するビュー

    - PaymentListAdapter

      - 収支情報の一覧を管理するクラス

- Controller

  - MainActivity

    - 収支登録画面を管理するクラス
    - 入力された情報の登録や収支、カテゴリの取得を行う

  - IndexActivity

    - 収支検索画面を管理するクラス
    - 入力された情報をもとに収支情報の検索やカテゴリの取得を行う

- InputChecker

  - 正しく入力されているかをチェックするクラス
  - 全項目が入力されているかをチェックする
  - 日付，金額が正しく入力されているかをチェックする

- HTTPClient

  - データベースサーバへアクセスし，以下を行うクラス

    - 収支情報の登録
    - 収支情報の検索
    - 収支の取得
    - カテゴリ情報の取得

.. _adh-int-seq:

シーケンス
----------

- :ref:`adh-int-seq-registration`
- :ref:`adh-int-seq-index`

.. _adh-int-seq-registration:

収支を登録する
^^^^^^^^^^^^^^

.. uml:: umls/seq-registration.uml

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
12. setCategoriesメソッドを実行してダイアログにカテゴリ名をセットする
13. 利用者が登録ボタンを押下するとonClickメソッドが実行される
14. registPaymentメソッドを実行して入力情報を登録する
15. checkEmptyメソッドを実行して入力が空の項目がないかチェックする
16. 空の項目がある場合はshowMessageメソッドを実行してエラーメッセージを表示する
17. さらにshowWrontInputメソッドを実行して不正な入力項目にチェックマークを付ける
18. 空の項目がない場合は，checkDateメソッドを実行して入力された日付のフォーマットをチェックする
19. 日付が不正な場合は，showMessageメソッドを実行してエラーメッセージを表示する
20. さらにshowWrontInputメソッドを実行して日付入力項目にチェックマークを付ける
21. 日付のフォーマットが正しい場合は，checkPriceメソッドを実行して入力された金額をチェックする
22. 金額が不正な場合は，showMessageメソッドを実行してエラーメッセージを表示する
23. さらにshowWrontInputメソッドを実行して金額入力項目にチェックマークを付ける
24. 金額が正しい場合は，収支情報を登録するためのHTTPClientクラスのインスタンスを作成する
25. credentialメソッドを実行してWebAPIを利用するためのAuthorizationヘッダーをセットする
26. sendRequestメソッドを実行してWebAPIを実行し，収支情報を登録する
27. showMessageメソッドを実行して収支情報が登録された旨を通知する
28. resetFieldメソッドを実行して入力フォームを空文字にする
29. setTodayメソッドを実行して日付入力欄にアプリ起動時の日付をセットする
30. settleメソッドを実行して収支を取得する
31. 収支を取得するためにHTTPClientクラスのインスタンスを作成する
32. credentialメソッドを実行してWebAPIを利用するためのAuthorizationヘッダーをセットする
33. sendRequestメソッドを実行してWebAPIを実行し，収支を取得する
34. showSettlementメソッドを実行して今月の収支を表示する

.. _adh-int-seq-index:

収支を検索する
^^^^^^^^^^^^^^

.. uml:: umls/seq-index.uml

1. 利用者がメニューから「検索」を選択するとonOptionsItemSelectedメソッドが実行される
2. アプリはsetContentViewメソッドを実行して検索画面に切り替える
3. アプリはgetCategoriesメソッドを実行してカテゴリ一覧を取得する
4. カテゴリ情報を取得するためにHTTPClientクラスのインスタンスを作成する
5. credentialメソッドを実行してWebAPIを利用するためのAuthorizationヘッダーをセットする
6. sendRequestメソッドを実行してWebAPIを実行し，カテゴリ情報を取得する
7. setCategoriesメソッドを実行してCategoryViewにカテゴリ情報をセットする
8. setCategoriesメソッドを実行してダイアログにカテゴリ名をセットする
9. 利用者が検索ボタンを押下するとonClickメソッドが実行される
10. searchPaymentsメソッドを実行して収支情報を検索する
11. checkDateメソッドを実行して入力された日付のフォーマットをチェックする
12. 日付が不正な場合は，showMessageメソッドを実行してエラーメッセージを表示する
13. さらにshowWrontInputメソッドを実行して日付入力項目にチェックマークを付ける
14. 日付のフォーマットが正しい場合は，checkPriceメソッドを実行して入力された金額をチェックする
15. 金額が不正な場合は，showMessageメソッドを実行してエラーメッセージを表示する
16. さらにshowWrontInputメソッドを実行して金額入力項目にチェックマークを付ける
17. 金額が正しい場合は，収支情報を検索するためのHTTPClientクラスのインスタンスを作成する
18. credentialメソッドを実行してWebAPIを利用するためのAuthorizationヘッダーをセットする
19. sendRequestメソッドを実行してWebAPIを実行し，収支情報を検索する
20. addResultsメソッドを実行して取得した収支情報を表示する
21. setCurrentPageメソッドを実行して現在取得しているページ数を1にセットする
22. 利用者が「さらに読み込む」ボタンを押下するとonClickメソッドが実行される
23. searchPaymentsメソッドを実行して次のページの収支情報を取得する
24. 収支情報を検索するためのHTTPClientクラスのインスタンスを作成する
25. credentialメソッドを実行してWebAPIを利用するためのAuthorizationヘッダーをセットする
26. sendRequestメソッドを実行してWebAPIを実行し，収支情報を検索する
27. addResultsメソッドを実行して取得した収支情報を表示する
28. getCurrentPageメソッドを実行して現在取得しているページ数を取得する
29. setCurrentPageメソッドを実行して取得したページ数を1を加算した値をセットする

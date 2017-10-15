設計仕様
========

設計仕様では以下を定義する

- :ref:`alg-int-cls`
- :ref:`alg-int-seq`

.. _alg-int-cls:

モジュール構成
--------------

MVCモデルを利用する

*クラス図*

.. uml:: umls/class.uml

- Model

  - 画面から分析を実行するアプリのためモデルは存在しない

- View

  - Analyze_View: 分析を行うビュー

    - 利用者が分析を実行するための画面

- Controller

  - 画面から分析を実行するアプリのためモデルは存在しない

.. _alg-int-seq:

シーケンス
----------

- :ref:`alg-int-seq-analyze`

.. _alg-int-seq-analyze:

過去のレースを分析する
^^^^^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-analyze.uml

1. 利用者がブラウザから本アプリにアクセスする
2. 利用者がユーザーIDとパスワードを入力してログインする
3. LoginControllerがユーザーIDとパスワードが一致するUserオブジェクトを検索する
4. 一致するユーザーが存在しなければLogin_Viewを表示して2へ戻る
5. 一致するユーザーが存在すればPaymentController#manageを実行する
6. PaymentControllerがPaymentを取得してPayment_Viewを表示する

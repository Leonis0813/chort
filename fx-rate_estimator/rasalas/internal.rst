設計仕様
========

設計仕様では以下を定義する

- :ref:`ras-int-cls`
- :ref:`ras-int-seq`

.. _ras-int-cls:

モジュール構成
--------------

MVCモデルを利用する

*クラス図*

.. uml:: umls/class.uml

- Model

  - Prediction

    - 予測結果を管理するクラス

- View

  - PredictionResultView

    - 予測結果を確認するためのビューを表すクラス
    - 下記のビューを含む

      - PredictionListAdapter

        - 予測結果の一覧を管理するクラス

- Controller

  - MainActivity

    - 予測結果を管理するコントローラー

- HTTPClient

  - データベースサーバーへアクセスして予測結果を取得するクラス

.. _ras-int-seq:

シーケンス
----------

- :ref:`ras-int-seq-confirm`

.. _ras-int-seq-confirm:

予測結果を確認する
^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-confirm.uml

1. 利用者がアプリを起動する
2. searchPredictions メソッドを実行して予測結果を取得する
3. HTTPClient クラスのオブジェクトを作成する
4. HTTPClient クラスのオブジェクトにアプリ認証ヘッダーを設定する
5. WebAPI を実行して予測結果を取得する
6. addPredictions メソッドを実行して画面に予測結果を表示する
7. setAdapter メソッドを実行して予測結果をビューにセットする

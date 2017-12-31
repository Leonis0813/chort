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

1. 利用者がパラメーターを入力して実行ボタンを押下する
2. Analyze_ViewがAnalysisControllerのlearnメソッドを実行する
3. AnalysisControllerが非同期でAnalysisJobのperform_laterを実行した後，利用者に分析が実行されたことを通知する
4. AnalysisJobが分析を完了させた後，AnalysisMailerのfinishedを実行して利用者にメールを送信する

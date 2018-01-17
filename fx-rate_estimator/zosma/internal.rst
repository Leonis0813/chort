設計仕様
========

設計仕様では以下を定義する

- :ref:`zos-int-cls`
- :ref:`zos-int-seq`
- :ref:`zos-int-sch`

.. _zos-int-cls:

モジュール構成
--------------

*クラス図*

.. uml:: umls/class.uml

- Rate

  - レートを表すクラス

- CandleStick

  - ローソク足を表すクラス

.. _zos-int-seq:

シーケンス
----------

- :ref:`zos-int-seq-collect`
- :ref:`zos-int-seq-aggregate`

.. _zos-int-seq-collect:

レートを収集する
^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-collect.uml

1. レートのファイル数分MysqlClientクラスのexecute_queryメソッドを実行してレートをDBに登録する

   - 外部ツールのレートはリアルタイムで日付・ペアごとにファイル出力されている

.. _zos-int-seq-aggregate:

指標を生成する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-aggregate.uml

1. 1分間隔でペアごとにレートから1日分のローソク足を作成する

機能仕様
========

機能仕様では以下を定義する

- :ref:`den-ext-resource`
- :ref:`den-ext-api`

.. _den-ext-resource:

リソース
--------

本システムでは以下のリソースを扱う

- :ref:`den-ext-resource-races`
- :ref:`den-ext-resource-entries`
- :ref:`den-ext-resource-results`
- :ref:`den-ext-resource-features`

.. _den-ext-resource-races:

レース
^^^^^^

レース情報を表す

.. csv-table::
   :header: "属性名", "型", "意味", "備考"
   :widths: 15, 10, 45, 30

   "方向", "文字列", "レースが左回りか右回りか", "- 以下のいずれか

   - 左
   - 右"
   "距離", "数値", "コースの距離", "- 0より大きい整数"
   "グレード", "文字列", "レースのグレード", "- 以下のいずれか

     - G1
     - G2
     - G3

   - デフォルト null"
   "場所", "文字列", "レースの開催場所", "- 以下のいずれか

     - 中京
     - 中山
     - 京都
     - 函館
     - 小倉
     - 新潟
     - 札幌
     - 東京
     - 福島
     - 阪神"
   "ラウンド", "数値", "その日の何度目のレースか", "- 0より大きい整数"
   "開催日時", "日時", "レースの開催日時", "- 年/月/日 時:分:秒"
   "地面の種類", "文字列", "コースの地面の種類", "- 以下のいずれか

     - 芝
     - ダ"
   "天候", "文字列", "レース開催日の天候", "- 以下のいずれか"

     - 晴
     - 曇
     - 小雨
     - 雨
     - 小雪
     - 雪"

.. _den-ext-resource-entries:

エントリー
^^^^^^^^^^


.. csv-table::
   :header: "属性名", "型", "意味", "フォーマット"
   :widths: 15, 10, 45, 30

.. _den-ext-resource-results:

レース結果
^^^^^^^^^^

.. csv-table::
   :header: "属性名", "型", "意味", "フォーマット"
   :widths: 15, 10, 45, 30

.. _den-ext-resource-features:

素性
^^^^

.. csv-table::
   :header: "属性名", "型", "意味", "フォーマット"
   :widths: 15, 10, 45, 30

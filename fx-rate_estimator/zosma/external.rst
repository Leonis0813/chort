機能仕様
========

機能仕様では以下を定義する

- :ref:`zos-ext-resource`
- :ref:`zos-ext-api`

.. _zos-ext-resource:

リソース
--------

本システムでは以下のリソースを扱う

- :ref:`zos-ext-res-rates`
- :ref:`zos-ext-res-candle-sticks`
- :ref:`zos-ext-res-moving-averages`

.. _zos-ext-res-rates:

レート
^^^^^^

FXにおけるレート値を表す

.. csv-table::
   :header: "属性名", "型", "意味", "備考"
   :widths: 15, 10, 45, 30

   "時刻", "日時", "そのレート値になった時刻", "- 年/月/日 時:分:秒"
   "通貨ペア", "文字列", "どの国の通貨の相場かを表すコード", "- :ref:`zos-ext-res-pair` 参照"
   "買値", "数値", "通貨を買う時の値段", "- 0より大きい小数値"
   "売値", "数値", "通貨を売る時の値段", "- 0より大きい小数値"

.. _zos-ext-res-candle-sticks:

ローソク足
^^^^^^^^^^

ローソク足チャートにおける1本のバーを表す

.. csv-table::
   :header: "属性名", "型", "意味", "備考"
   :widths: 15, 10, 45, 30

   "開始日時", "日時", "バーが集約した期間の最初の時刻", "- 年/月/日 時:分:秒"
   "終了日時", "日時", "バーが集約した期間の最後の時刻", "- 年/月/日 時:分:秒"
   "通貨ペア", "文字列", "どの国の通貨の相場かを表すコード", "- :ref:`zos-ext-res-pair` 参照"
   "時間枠", "文字列", "バーが集約した期間を表すコード", "- 半角英数字と記号"
   "始値", "数値", "開始日時での相場", "- 0以上の小数"
   "終値", "数値", "終了日時での相場", "- 0以上の小数"
   "高値", "数値", "最もレート値が大きくなった時の値", "- 0以上の小数"
   "安値", "数値", "最もレート値が小さくなった時の値", "- 0以上の小数"

.. _zos-ext-res-moving-averages:

移動平均
^^^^^^^^

ある時間における移動平均値を表す

.. csv-table::
   :header: "属性名", "型", "意味", "フォーマット"
   :widths: 15, 10, 45, 30

   "日時", "日時", "移動平均を算出した日時", "- 年/月/日 時:分:秒"
   "通貨ペア", "文字列", "どの国の通貨の相場かを表すコード", "- :ref:`zos-ext-res-pair` 参照"
   "時間枠", "文字列", "移動平均値の算出に使用したバーが集約した期間を表すコード", "- 半角英数字と記号"
   "期間", "数値", "移動平均値の算出に使用した期間", "- 0以上の整数"
   "移動平均値", "数値", "移動平均の値", "- 0以上の小数"

.. _zos-ext-res-pair:

通貨ペア
^^^^^^^^

本システムでは以下の通貨ペアに対応している

- AUDJPY
- CADJPY
- CHFJPY
- EURJPY
- EURUSD
- GBPJPY
- NZDJPY
- USDJPY

.. _zos-ext-api:

インターフェース
----------------

本システムは以下の機能を備えている

- :ref:`zos-ext-api-import-rates`
- :ref:`zos-ext-api-import-candle-sticks`
- :ref:`zos-ext-api-import-moving-averages`

また，以下のツールを提供する

- :ref:`zos-ext-api-tool-backup`
- :ref:`zos-ext-api-tool-compress`
- :ref:`zos-ext-api-tool-remove`

.. _zos-ext-api-import-rates:

レートを収集する
^^^^^^^^^^^^^^^^

- 外部ツールからレート情報を収集し，データベースに :ref:`zos-ext-res-rates` を登録する
- 指定された期間のレートを収集する

**スクリプト**

import/rates.rb

**入力**

- 収集開始日

  - 指定がなければ2日前の日付となる

- 収集終了日

  - 指定がなければスクリプトを実行した日付となる

**出力**

- :ref:`zos-ext-res-rates`

**実行例**

  .. code-block:: none

     bundle exec ruby import/rates.rb --from=2018-01-01 --to=2018-01-31

.. _zos-ext-api-import-candle-sticks:

ローソク足を収集する
^^^^^^^^^^^^^^^^^^^^

- 外部ツールからローソク足情報を収集し，データベースに :ref:`zos-ext-res-candle-sticks` を登録する
- 指定された期間のローソク足を収集する

**スクリプト**

import/candle_sticks.rb

**入力**

- 収集開始日

  - 指定がなければ2日前の日付となる

- 収集終了日

  - 指定がなければスクリプトを実行した日付となる

**出力**

- :ref:`zos-ext-res-candle-sticks`

**実行例**

  .. code-block:: none

     bundle exec ruby import/candle_sticks.rb --from=2018-01-01 --to=2018-01-31

.. _zos-ext-api-import-moving-averages:

移動平均を収集する
^^^^^^^^^^^^^^^^^^

- 外部ツールから移動平均情報を収集し，データベースに :ref:`zos-ext-res-moving-averages` を登録する
- 指定された期間の移動平均を収集する

**スクリプト**

import/moving_averages.rb

**入力**

- 収集開始日

  - 指定がなければ2日前の日付となる

- 収集終了日

  - 指定がなければスクリプトを実行した日付となる

**出力**

- :ref:`zos-ext-res-moving-averages`

**実行例**

  .. code-block:: none

     bundle exec ruby import/moving_averages.rb --from=2018-01-01 --to=2018-01-31

.. _zos-ext-api-tool-backup:

バックアップを取得する
^^^^^^^^^^^^^^^^^^^^^^

- データベースからレート情報をCSVファイルに出力する
- 指定された期間に対して1日単位でファイルを生成する

**スクリプト**

tools/backup.rb

**入力**

- バックアップ開始日

  - 指定がなければ2日前の日付となる

- バックアップ終了日

  - 指定がなければスクリプトを実行した日付となる

**出力**

- CSVファイル

  - YYYY-MM-DD.csvというフォーマットの名前でファイルを生成する
  - ファイルに出力するデータは各リソースに依存する

**実行例**

  .. code-block:: none

     bundle exec ruby tools/backup.rb --from=2018-01-01 --to=2018-01-31

.. _zos-ext-api-tool-compress:

バックアップファイルを圧縮する
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- バックアップされているCSVファイルをtar.gz形式で圧縮する
- スクリプトを実行した月の前の月のバックアップファイルを圧縮する

**スクリプト**

tools/compress.rb

**入力**

なし

**出力**

- 圧縮ファイル

  - YYYY-MM.tar.gzというフォーマットの名前でファイルを生成する
  - 圧縮ファイルにはYYYY-MMというフォーマットのディレクトリがあり，その中にCSVが保存されている

**実行例**

  .. code-block:: none

     bundle exec ruby tools/compress.rb

.. _zos-ext-api-tool-remove:

外部ツールの出力ファイルを削除する
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- 外部ツールが出力しているCSVファイルを削除する
- スクリプトを実行した日の2日前に生成されたCSVファイルを削除する

**スクリプト**

tools/remove.rb

**入力**

なし

**出力**

なし

**実行例**

  .. code-block:: none

     bundle exec ruby tools/remove.rb

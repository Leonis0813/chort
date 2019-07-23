機能仕様
========

機能仕様では以下を定義する

- :ref:`den-ext-resource`
- :ref:`den-ext-api`

.. _den-ext-resource:

リソース
--------

本システムでは以下のリソースを扱う

- :ref:`den-ext-res-races`
- :ref:`den-ext-res-entries`
- :ref:`den-ext-res-horses`
- :ref:`den-ext-res-payoffs`
- :ref:`den-ext-res-features`

.. _den-ext-res-races:

レース
^^^^^^

レース情報を表す

.. csv-table::
   :header: 属性名,型,意味,備考
   :widths: 15,10,30,45

   レースID,文字列,レースを一意に示すID,- 半角数字
   方向,文字列,レースが左回りか右回りか,"- 以下のいずれか

     - 左
     - 右
     - 直
     - 障"
   距離,数値,コースの距離,- 0より大きい整数
   グレード,文字列,レースのグレード,"- 以下のいずれか

     - G1
     - G2
     - G3
     - G
     - J.G1
     - J.G2
     - J.G3
     - L
     - OP
     - N

   - デフォルト N"
   場所,文字列,レースの開催場所,"- 以下のいずれか

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
   ラウンド,数値,その日の何度目のレースか,- 0より大きい整数
   開催日時,日時,レースの開催日時,- 年/月/日 時:分:秒
   地面の種類,文字列,コースの地面の種類,"- 以下のいずれか

     - 芝
     - ダート
     - 障"
   天候,文字列,レース開催日の天候,"- 以下のいずれか

     - 晴
     - 曇
     - 小雨
     - 雨
     - 小雪
     - 雪"

.. _den-ext-res-entries:

エントリー
^^^^^^^^^^

レースのエントリーを表す

.. csv-table::
   :header: 属性名,型,意味,備考
   :widths: 15,10,30,45

   年齢,数値,レース参加時の年齢,- 0より大きい整数
   性別,文字列,エントリーの性別,"- 以下のいずれか

     - 牝
     - 牡
     - セ"
   斤量,数値,エントリーの斤量,- 0より大きい小数
   騎手,文字列,騎手の名前,
   馬番,数値,エントリーの番号,- 0より大きい整数
   馬体重,数値,エントリー時の馬体重,- 0より大きい整数
   体重の差分,数値,前走との馬体重の差分,- 小数
   着順,文字列,レースで何番目にゴールに着いたか,"- 以下のいずれか

     - 1から18の半角数字
     - 除
     - 中
     - 取
     - 失"
   上り3ハロンタイム,数値,上り3ハロンタイム,- 0より大きい小数
   賞金,数値,獲得賞金,- 0以上の整数

.. _den-ext-res-horses:

競走馬
^^^^^^

競走馬を表す

.. csv-table::
   :header: 属性名,型,意味,備考
   :widths: 15,10,30,45

   競走馬ID,文字列,競走馬を一意に示すID,- 半角数字
   脚質,文字列,脚質,"- 以下のいずれか

     - 逃げ
     - 先行
     - 差し
     - 追込"

.. _den-ext-res-payoffs:

払い戻し
^^^^^^^^

レースの払い戻しを表す

.. csv-table::
   :header: 属性名,型,意味,備考
   :widths: 15,10,30,45

   レースID,文字列,レースを一意に示すID,- 半角数字
   馬券,文字列,馬券の種類,"- 以下のいずれか

     - 単勝
     - 複勝
     - 枠連
     - 馬連
     - ワイド
     - 馬単
     - 三連複
     - 三連単"
   オッズ,数値,オッズ,- 1より大きい数値

.. _den-ext-res-features:

素性
^^^^

レースの分析に利用する特徴量を表す

.. csv-table::
   :header: 属性名,型,意味,備考
   :widths: 15,10,30,45

   方向,文字列,レースが左回りか右回りか,- :ref:`den-ext-res-races` 参照
   距離,数値,コースの距離,- :ref:`den-ext-res-races` 参照
   グレード,文字列,レースのグレード,- :ref:`den-ext-res-races` 参照
   場所,文字列,レースの開催場所,- :ref:`den-ext-res-races` 参照
   ラウンド,数値,その日の何度目のレースか,- :ref:`den-ext-res-races` 参照
   地面の種類,文字列,コースの地面の種類,- :ref:`den-ext-res-races` 参照
   天候,文字列,レース開催日の天候,- :ref:`den-ext-res-races` 参照
   年齢,数値,レース参加時の年齢,- :ref:`den-ext-res-entries` 参照
   性別,文字列,エントリーの性別,- :ref:`den-ext-res-entries` 参照
   斤量,数値,エントリーの斤量,- :ref:`den-ext-res-entries` 参照
   馬番,数値,エントリーの番号,- :ref:`den-ext-res-entries` 参照
   馬体重,数値,エントリー時の馬体重,- :ref:`den-ext-res-entries` 参照
   体重の差分,数値,前走との馬体重の差分,- :ref:`den-ext-res-entries` 参照
   脚質,文字列,馬の脚質,- :ref:`den-ext-res-horses` 参照
   開催月,数値,レースの開催月,- 0より大きい整数
   平均距離との差分,数値,平均距離との差/平均距離,- 0以上の小数
   空き日数,数値,前回のレースから何日空いたか,"- 0以上の整数
   - 前回のレースがない場合は0となる"
   斤量比,数値,斤量/馬体重,- 0より大きい小数
   前走の着順,数値,馬の1走前の順位,- :ref:`den-ext-res-entries` 参照
   2走前の着順,数値,馬の2走前の順位,- :ref:`den-ext-res-entries` 参照
   3着以内の割合,数値,馬の過去4レースの3着以内に入っていた割合,- 0以上の小数
   出場回数,数値,レースの出場回数,- 0以上の整数
   平均獲得賞金額,数値,馬の平均賞金獲得額,- 0以上の小数
   勝利数,数値,馬の勝ち回数,- 0以上の整数
   ラベル,真偽値,レースに勝ったかどうか,- true, falseのいずれか

.. _den-ext-api:

インターフェース
----------------

本システムは以下の機能を備えている

- :ref:`den-ext-api-collect`
- :ref:`den-ext-api-extract`
- :ref:`den-ext-api-aggregate`

.. _den-ext-api-collect:

HTMLファイルを収集する
^^^^^^^^^^^^^^^^^^^^^^

- 外部サイトから競馬情報が掲載されているウェブページにアクセスしてHTMLファイルを収集する
- 指定された期間の競馬情報を収集する

**スクリプト**

collect.rb

**入力**

- 収集開始日

  - 指定がなければ実行した日の30日前の日付となる
  - 日付はyyyy-mm-ddの形式で指定する

- 収集終了日

  - 指定がなければ実行した日付となる
  - 日付はyyyy-mm-ddの形式で指定する

**出力**

- ファイル

**実行例**

  .. code-block:: none

     bundle exec ruby collect.rb --from=2018-01-01 --to=2018-01-31

.. _den-ext-api-extract:

競馬情報を抽出する
^^^^^^^^^^^^^^^^^^

- HTMLファイルから以下の情報を抽出してデータベースに保存する

  - :ref:`den-ext-res-races`
  - :ref:`den-ext-res-entries`
  - :ref:`den-ext-res-horses`
  - :ref:`den-ext-res-payoffs`

- 指定した期間の競馬情報を抽出する

**スクリプト**

extract.rb

**入力**

- 収集開始日

  - 指定がなければ実行した日の30日前の日付となる
  - 日付はyyyy-mm-ddの形式で指定する

- 収集終了日

  - 指定がなければ実行した日付となる
  - 日付はyyyy-mm-ddの形式で指定する

**出力**

- :ref:`den-ext-res-races`
- :ref:`den-ext-res-entries`
- :ref:`den-ext-res-horses`
- :ref:`den-ext-res-payoffs`

**実行例**

  .. code-block:: none

     bundle exec ruby extract.rb --from=2018-01-01 --to=2018-01-31

.. _den-ext-api-aggregate:

リソースを集約する
^^^^^^^^^^^^^^^^^^

抽出したリソースを集約して素性を生成する

**スクリプト**

aggregate.rb

**入力**

- なし

**出力**

- :ref:`den-ext-res-features`

**実行例**

  .. code-block:: none

     bundle exec ruby aggregate.rb

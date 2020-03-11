設計仕様
========

設計仕様では以下を定義する

- :ref:`alg-int-cls`
- :ref:`alg-int-seq`
- :ref:`alg-int-scm`

.. _alg-int-cls:

モジュール構成
--------------

MVCモデルを利用する

*クラス図*

.. uml:: umls/class.uml

- Model

  - Category

    - categoriesテーブルを操作するモデル

  - Dictionary

    - dictionariesテーブルを管理するモデル

  - Payment

    - paymentsテーブルを操作するモデル

  - Query

    - クエリを管理するモデル
    - 以下のサブクラスを持つ

      - CategoryQuery

        - カテゴリ検索時のクエリを管理する

      - DictionaryQuery

        - 辞書検索時のクエリを管理する

      - PaymentQuery

        - 収支検索時のクエリを管理する

      - TagQuery

        - タグ検索時のクエリを管理する

  - Settlement

    - 収支計算時のクエリを管理するモデル

  - Tag

    - tagsテーブルを管理するモデル

- View

  - ManagementView

    - 管理画面を表すビュー
    - 以下のサブビューを持つ

      - CategoryView

        - カテゴリ情報を管理するビュー

      - DictionaryView

        - 辞書情報を管理するビュー

      - PaymentView

        - 収支情報を管理するビュー

      - TagView

        - タグ情報を管理するビュー

  - StatisticsView

    - 統計画面を表すビュー

- Controller

  - Api::CategoriesController

    - カテゴリを処理するコントローラー
    - WebAPI用コントローラー

  - Api::DictionariesController

    - 辞書を処理するコントローラー
    - WebAPI用コントローラー

  - Api::PaymentsController

    - 収支を処理するコントローラー
    - WebAPI用コントローラー

  - Api::TagsController

    - タグを処理するコントローラー
    - WebAPI用コントローラー

  - CategoriesController

    - カテゴリを処理するコントローラー
    - UI用コントローラー

  - DictionariesController

    - 辞書を処理するコントローラー
    - UI用コントローラー

  - PaymentsController

    - 収支を処理するコントローラー
    - UI用コントローラー

  - TagsController

    - タグを処理するコントローラー
    - UI用コントローラー

  - StatisticsController

    - 統計情報を管理するコントローラー
    - UI用コントローラー

.. _alg-int-seq:

シーケンス
----------

- :ref:`alg-int-seq-create-payment`
- :ref:`alg-int-seq-index-payments`
- :ref:`alg-int-seq-destroy-payment`
- :ref:`alg-int-seq-index-categories`
- :ref:`alg-int-seq-create-dictionary`
- :ref:`alg-int-seq-index-dictionaries`
- :ref:`alg-int-seq-create-tag`
- :ref:`alg-int-seq-assign-tag`
- :ref:`alg-int-seq-index-tags`
- :ref:`alg-int-seq-statistics`

.. _alg-int-seq-create-payment:

収支を登録する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-create-payment.uml

1. 利用者が管理画面を表示する
2. ブラウザが管理画面の表示を要求する
3. 利用者が内容を入力する
4. 管理画面が辞書情報を検索するAPIを実行する
5. 入力された内容にマッチする辞書情報を検索する
6. 利用者がフォームに入力して登録ボタンを押下する
7. 管理画面が収支情報を登録するAPIを実行する
8. 必須パラメーターが指定されているかチェックする

必須パラメーターがない場合はエラーを表示して終了する

9. 入力されたパラメーターから収支情報を作成する

入力されたカテゴリの数だけ10を実行する

10. カテゴリ情報を取得する．なければオブジェクトを作成する

入力されたカテゴリの数だけ11を実行する

11. タグ情報を取得する．なければオブジェクトを作成する

12. 収支情報をデータベースに登録する

登録に成功した場合は13〜17を実行する

13. 登録された収支情報で辞書情報を検索する

該当する辞書情報が登録されていない場合は14〜16を実行する

14. 辞書情報を登録するためのダイアログを表示する
15. 利用者が登録ボタンを押下する
16. 管理画面が辞書情報を登録するAPIを実行する

17. 管理画面をリロードする

登録に失敗した場合はエラーを表示して終了する

.. _alg-int-seq-index-payments:

収支を検索する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-index-payments.uml

1. 利用者が検索条件をフォームに入力して検索ボタンを押下する
2. クエリを指定して管理画面の再表示を要求する
3. 指定されたパラメーターからクエリ情報を作成する
4. クエリ情報が不正でないか確認する

クエリ情報が不正な場合はエラーを表示して終了する

5. クエリを満たす収支情報を検索する

.. _alg-int-seq-destroy-payment:

収支を削除する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-destroy-payment.uml

1. 利用者が収支情報を選択して削除ボタンを押下する
2. 管理画面が収支情報を削除するAPIを実行する
3. 指定された収支情報が存在するかチェックする

指定された収支情報が存在しない場合はエラーを表示して終了する

4. 指定された収支情報を削除する
5. 管理画面をリロードする

.. _alg-int-seq-index-categories:

カテゴリを検索する
^^^^^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-index-categories.uml

1. 利用者が検索条件をフォームに入力して検索ボタンを押下する
2. クエリを指定して管理画面の再表示を要求する
3. 指定されたパラメーターからクエリ情報を作成する
4. クエリ情報が不正でないか確認する

クエリ情報が不正な場合はエラーを表示して終了する

5. クエリを満たすカテゴリ情報を検索する

.. _alg-int-seq-create-dictionary:

辞書を登録する
^^^^^^^^^^^^^^

.. uml:: umls/seq-create-dictionary.uml

1. 利用者が辞書情報をフォームに入力して登録ボタンを押下する
2. 管理画面が辞書情報を登録するAPIを実行する
3. 必須パラメーターが指定されているかチェックする

必須パラメーターがない場合はエラーを表示して終了する

4. 指定されたパラメーターから辞書情報を作成する

指定されたカテゴリ名の数だけ5を実行する

5. データベースからカテゴリ情報を取得する．なければオブジェクトを作成する

6. 辞書情報をデータベースに登録する

.. _alg-int-seq-index-dictionaries:

辞書を検索する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-index-dictionaries.uml

1. 利用者が検索条件をフォームに入力して検索ボタンを押下する
2. クエリを指定して管理画面の再表示を要求する
3. 指定されたパラメーターからクエリ情報を作成する
4. クエリ情報が不正でないか確認する

クエリ情報が不正な場合はエラーを表示して終了する

5. クエリを満たす辞書情報を検索する

.. _alg-int-seq-create-tag:

タグを登録する
^^^^^^^^^^^^^^

.. uml:: umls/seq-create-tag.uml

1. 利用者がタグ情報をフォームに入力して登録ボタンを押下する
2. 管理画面がタグ情報を登録するAPIを実行する
3. 必須パラメーターが指定されているかチェックする

必須パラメーターがない場合はエラーを表示して終了する

4. 指定されたパラメーターからタグ情報を作成する
5. タグ情報をデータベースに登録する

.. _alg-int-seq-assign-tag:

タグを設定する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-assign-tag.uml

1. 利用者が設定する収支とタグ情報を入力して設定ボタンを押下する
2. 管理画面がタグを設定するAPIを実行する
3. 指定されたタグが存在するかチェックする

指定されたタグが存在しない場合はエラーを表示して終了する

4. 必須パラメーターが指定されているかチェックする

必須パラメーターがない場合はエラーを表示して終了する

5. 指定された条件を満たす収支情報を検索する
6. 取得した収支情報にタグを設定する

.. _alg-int-seq-index-tags:

タグを検索する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml:: umls/seq-index-tags.uml

1. 利用者が検索条件をフォームに入力して検索ボタンを押下する
2. クエリを指定して管理画面の再表示を要求する
3. 指定されたパラメーターからクエリ情報を作成する
4. クエリ情報が不正でないか確認する

クエリ情報が不正な場合はエラーを表示して終了する

5. クエリを満たすタグ情報を検索する

.. _alg-int-seq-statistics:

統計情報を表示する
^^^^^^^^^^^^^^^^^^

.. uml:: umls/seq-statistics.uml

1. 利用者が統計画面を表示する
2. 統計画面が統計情報の表示を要求する
3. 統計画面が期間別に収支を計算するAPIを実行する
4. 収支情報から収支を計算する
5. 統計画面がカテゴリ別に収入の割合を計算するAPIを実行する
6. 収支情報から割合を計算する
7. 統計画面がカテゴリ別に支出の割合を計算するAPIを実行する
8. 収支情報から割合を計算する
9. 利用者がグラフをクリックする
10. 統計画面が期間別に収支を計算するAPIを実行する
11. 収支情報から収支を計算する

.. _alg-int-scm:

データベース構成
----------------

データベースは下記のテーブルで構成される

- :ref:`alg-int-scm-categories`
- :ref:`alg-int-scm-category-dictionaries`
- :ref:`alg-int-scm-category-payments`
- :ref:`alg-int-scm-dictionaries`
- :ref:`alg-int-scm-payments`
- :ref:`alg-int-scm-payment-tags`
- :ref:`alg-int-scm-tags`

.. _alg-int-scm-categories:

categories テーブル
^^^^^^^^^^^^^^^^^^^

カテゴリ情報を登録するcategoriesテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL

   id,INTEGER,内部ID,○
   category_id,STRING,カテゴリを一意に示すID,
   name,STRING,カテゴリの名前,○
   description,STRING,カテゴリの説明,
   created_at,DATETIME,カテゴリ情報の作成日時,○
   updated_at,DATETIME,カテゴリ情報の更新日時,○

.. _alg-int-scm-category-dictionaries:

category_dictionaries テーブル
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

カテゴリ情報と辞書情報を紐づける中間テーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL

   id,INTEGER,内部ID,○
   category_id,INTEGER,categoriesテーブルの内部ID,○
   dictionary_id,INTEGER,dictionariesテーブルの内部ID,○
   created_at,DATETIME,レコードの作成日時,○
   updated_at,DATETIME,レコードの更新日時,○

.. _alg-int-scm-category-payments:

category_payments テーブル
^^^^^^^^^^^^^^^^^^^^^^^^^^

カテゴリ情報と収支情報を紐づける中間テーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL

   id,INTEGER,内部ID,○
   category_id,INTEGER,categoriesテーブルの内部ID,○
   payment_id,INTEGER,paymentsテーブルの内部ID,○
   created_at,DATETIME,レコードの作成日時,○
   updated_at,DATETIME,レコードの更新日時,○

.. _alg-int-scm-dictionaries:

dictionaries テーブル
^^^^^^^^^^^^^^^^^^^^^

辞書情報を登録するcategoriesテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL

   id,INTEGER,内部ID,○
   dictionary_id,STRING,辞書を一意に示すID,
   phrase,STRING,フレーズ,○
   condition,STRING,条件,○
   created_at,DATETIME,辞書情報の登録日時,○
   updated_at,DATETIME,辞書情報の更新日時,○

.. _alg-int-scm-payments:

payments テーブル
^^^^^^^^^^^^^^^^^

収支情報を登録するpaymentsテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL

   id,INTEGER,内部ID,○
   payment_id,STRING,収支を一意に示すID,
   payment_type,STRING,収支の種類,○
   date,DATE,収入/支出があった日,○
   content,STRING,収入/支出の内容,○
   price,INTEGER,収入/支出の金額,○
   created_at,DATETIME,収支情報の登録日時,○
   updated_at,DATETIME,収支情報の更新日時,○

.. _alg-int-scm-payment-tags:

payment_tags テーブル
^^^^^^^^^^^^^^^^^^^^^

タグ情報と収支情報を紐づける中間テーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL

   id,INTEGER,内部ID,○
   payment_id,INTEGER,paymentsテーブルの内部ID,○
   tag_id,INTEGER,tagsテーブルの内部ID,○
   created_at,DATETIME,レコードの作成日時,○
   updated_at,DATETIME,レコードの更新日時,○

.. _alg-int-scm-tags:

tags テーブル
^^^^^^^^^^^^^

タグ情報を登録するtagsテーブルを定義する

.. csv-table::
   :header: カラム,型,内容,NOT NULL

   id,INTEGER,内部ID,○
   tag_id,STRING,タグを一意に示すID,
   name,STRING,タグ名,○
   created_at,DATETIME,タグ情報の登録日時,○
   updated_at,DATETIME,タグ情報の更新日時,○

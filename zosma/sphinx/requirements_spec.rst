要求仕様
========

本モジュールは以下の１つの機能を提供する

- `家計簿の検索 <http://localhost/zosma_docs/requirements_spec.html#id2>`__

*ユースケース図*

.. uml::

   left to right direction
   skinparam packageStyle rect
   actor 利用者
   rectangle 家計簿管理システム {
     利用者 -- (家計簿を検索する)
   }

家計簿の検索
------------

- 検索条件を入力すると条件を満たす家計簿をサーバから取得する
- サーバから取得した家計簿情報を画面に表示する

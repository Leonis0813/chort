要求仕様
========

本モジュールは以下の１つの機能を提供する

- :ref:`zos-req-index`

*ユースケース図*

.. uml::

   left to right direction
   skinparam packageStyle rect
   actor 利用者
   rectangle 収支管理システム {
     利用者 -- (収支を検索する)
   }

.. _zos-req-index:

収支の検索
----------

- 検索条件を入力すると条件を満たす収支をサーバから取得する
- サーバから取得した収支情報を画面に表示する

PaymentManager: 収支情報管理サービス
====================================

本書では収支情報管理サービスの仕様を記載する

サービス全体構成
----------------

.. uml::

   left to right direction
   skinparam packageStyle rect
   actor 利用者
   entity データベース

   rectangle Adhafera {
     boundary 登録画面
     boundary 検索画面
     利用者 -- 登録画面
     利用者 -- 検索画面
   }

   boundary Webブラウザ
   利用者 -- Webブラウザ

   rectangle Algieba {
     登録画面 -- 収支情報管理API
     登録画面 -- カテゴリ情報管理API
     登録画面 -- 辞書情報管理API
     登録画面 -- 統計情報管理API
     検索画面 -- 収支情報管理API
     検索画面 -- カテゴリ情報管理API
     Webブラウザ -- 管理画面
     Webブラウザ -- 統計画面
     収支情報管理API -- データベース
     カテゴリ情報管理API -- データベース
     辞書情報管理API -- データベース
     統計情報管理API -- データベース
     管理画面 -- データベース
     統計画面 -- データベース
   }

- 利用者は Android アプリ(Adhafera)、Webブラウザを使って収支を管理する
- 収支情報は全てサーバー内のデータベースで一括管理される

モジュール一覧
--------------

本サービスは下記のモジュール・アプリで構成される:

.. toctree::
   :maxdepth: 1

   algieba/index
   adhafera/index

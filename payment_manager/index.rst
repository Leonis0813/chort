Welcome to PaymentManager's documentation!
==========================================

本書では収支情報管理サービスの仕様を記載する

本サービスは下記のモジュール・アプリで構成される:

.. toctree::
   :maxdepth: 1

   algieba/index
   adhafera/index
   zosma/index

サービス全体構成
================

.. uml::

   left to right direction
   skinparam packageStyle rect
   actor 利用者
   entity 収支

   rectangle Adhafera {
     boundary 登録画面
     control 登録コントローラー
     利用者 -- 登録画面
     登録画面 -- 登録コントローラー
   }

   rectangle Zosma {
     boundary 検索画面
     control 検索コントローラー
     利用者 -- 検索画面
     検索画面 -- 検索コントローラー
   }

   boundary Webブラウザ
   利用者 -- Webブラウザ

   rectangle Algieba {
     登録コントローラー -- (収支を登録する)
     Webブラウザ -- (収支を登録する)
     (収支を登録する) -- 収支
     Webブラウザ -- (収支を取得する)
     (収支を取得する) -- 収支
     Webブラウザ -- (収支を検索する)
     検索コントローラー -- (収支を検索する)
     (収支を検索する) -- 収支
     (収支を更新する) -- 収支
     Webブラウザ -- (収支を削除する)
     (収支を削除する) -- 収支
     登録コントローラー -- (収支を計算する)
     (収支を計算する) -- 収支
   }

- 利用者は Android アプリの登録・検索画面、Webブラウザを使って収支を管理する
- 収支情報は全てサーバー内で一括管理される

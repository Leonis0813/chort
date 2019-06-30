FX Rate Estimator: レート予測システム
=====================================

本書ではFXにおけるレート予測システムの仕様を記載する

サービス全体構成
----------------

.. uml::

   left to right direction
   skinparam packageStyle rect
   actor 利用者
   boundary Webブラウザ
   entity レート情報
   entity 分析ジョブ
   entity 予測ジョブ

   利用者 -- Webブラウザ

   rectangle Regulus {
     control 分析ジョブコントローラー
     control 予測ジョブコントローラー
     Webブラウザ -- 分析ジョブコントローラー
     Webブラウザ -- 予測ジョブコントローラー
     分析ジョブコントローラー -- レート情報
     予測ジョブコントローラー -- レート情報
     分析ジョブコントローラー -- 分析ジョブ
     予測ジョブコントローラー -- 予測ジョブ
   }

   rectangle 外部ツール {
   }

   rectangle Zosma {
     control レート情報収集スクリプト
     レート情報収集スクリプト -- 外部ツール
     レート情報収集スクリプト -- レート情報
   }

- 利用者はWebブラウザからレート情報の分析を実行する
- レート情報は定期的にスクリプトによって収集される

モジュール一覧
--------------

本サービスは下記のモジュール・アプリで構成される:

.. toctree::
   :maxdepth: 1

   regulus/index
   zosma/index

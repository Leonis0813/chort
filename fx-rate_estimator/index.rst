Welcome to FX Rate Estimator's documentation!
=============================================

本書ではFXにおけるレート予測システムの仕様を記載する

本サービスは下記のモジュール・アプリで構成される:

.. toctree::
   :maxdepth: 1

   regulus/index
   zosma/index

サービス全体構成
================

.. uml::

   left to right direction
   skinparam packageStyle rect
   actor 利用者
   boundary Webブラウザ
   entity レート情報
   entity 分析ジョブ

   利用者 -- Webブラウザ

   rectangle Regulus {
     control 分析ジョブコントローラー
     Webブラウザ -- 分析ジョブコントローラー
     分析ジョブコントローラー -- レート情報
     分析ジョブコントローラー -- 分析ジョブ
   }

   rectangle Zosma {
     control レート情報収集スクリプト
     レート情報収集スクリプト -- レート情報
   }

- 利用者はWebブラウザからレート情報の分析を実行する
- レート情報は定期的にスクリプトによって収集される

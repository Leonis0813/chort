Horse-Race Estimator
====================

本書では競馬予測システムの仕様を記載する

本サービスは下記のモジュール・アプリで構成される:

.. toctree::
   :maxdepth: 1

   alterf/index
   denebola/index

サービス全体構成
----------------

.. uml::

   left to right direction
   skinparam packageStyle rect
   actor 利用者
   boundary Webブラウザ
   entity レース情報
   entity 分析ジョブ

   利用者 -- Webブラウザ

   rectangle Alterf {
     control 分析ジョブコントローラー
     Webブラウザ -- 分析ジョブコントローラー
     分析ジョブコントローラー -- レース情報
     分析ジョブコントローラー -- 分析ジョブ
   }

   rectangle Denebola {
     control レース情報収集スクリプト
     レース情報収集スクリプト -- レース情報
   }

- 利用者はWebブラウザからレース情報の分析を実行する
- レース情報は定期的にスクリプトによって収集される

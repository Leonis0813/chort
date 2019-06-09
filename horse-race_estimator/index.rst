Horse-Race Estimator: 競馬予測システム
======================================

本書では競馬予測システムの仕様を記載する

サービス全体構成
----------------

.. uml::

   left to right direction
   skinparam packageStyle rect
   actor 利用者
   boundary Webブラウザ
   entity レース情報
   entity 分析ジョブ
   entity 予測ジョブ
   entity 評価ジョブ

   利用者 -- Webブラウザ

   rectangle Alterf {
     control 分析ジョブコントローラー
     control 予測ジョブコントローラー
     control 評価ジョブコントローラー
     Webブラウザ -- 分析ジョブコントローラー
     Webブラウザ -- 予測ジョブコントローラー
     Webブラウザ -- 評価ジョブコントローラー
     分析ジョブコントローラー -- レース情報
     分析ジョブコントローラー -- 分析ジョブ
     予測ジョブコントローラー -- 予測ジョブ
     評価ジョブコントローラー -- 評価ジョブ
   }

   rectangle Denebola {
     control レース情報収集スクリプト
     レース情報収集スクリプト -- レース情報
   }

- 利用者はWebブラウザからレースの分析，予測，評価を実行する
- レース情報は定期的にスクリプトによって収集される

モジュール一覧
--------------

本サービスは下記のモジュール・アプリで構成される:

.. toctree::
   :maxdepth: 1

   alterf/index
   denebola/index

分析ジョブ情報管理API
=====================

本APIでは以下の機能を提供する

- :ref:`alt-ext-api-ana-download`

.. _alt-ext-api-ana-download:

学習データをダウンロードする
----------------------------

.. http:get:: /analyses/[id]/training_data

   :status 200:
      - 収支の取得に成功
      - 分析に使用したデータ、パラメータが記載されたファイル
   :status 404:
      - 収支の取得に失敗
      - 存在しないIDを指定

   **リクエスト例**

   .. sourcecode:: http

      GET /analyses/1/training_data HTTP/1.1

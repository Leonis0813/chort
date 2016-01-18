要求仕様
========

本システムは以下の３つの機能を提供する

- `為替レートの表示 <http://localhost/regulus_docs/requirements_spec.html#id2>`__
- `ツイートの表示 <http://localhost/regulus_docs/requirements_spec.html#id3>`__
- `記事の表示 <http://localhost/regulus_docs/requirements_spec.html#id4>`__

*ユースケース図*

.. image:: images/use_case.jpg
   :alt: ユースケース図

為替レートの表示
----------------

- 為替レートの変化を表したローソク足グラフをブラウザ上に表示する
- 為替のペアはUSDJPYで固定されている
- グラフは定期的に更新される

ツイートの表示
--------------

- Twitterから取得したツイートをブラウザ上に表示する
- ツイートは定期的に更新される

記事の表示
----------

- 日本経済新聞(日経)のRSSから取得した記事をブラウザ上に表示する
- 記事は定期的に表示される

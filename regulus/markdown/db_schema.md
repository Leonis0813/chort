# DBスキーマ定義
**通貨情報を登録するCurrencyテーブル，ツイートを登録するTweetテーブル，記事を登録するArticleテーブルを定義する**

- Currencyテーブル

|カラム |型     |内容               |主キー  |NOT NULL|
|------|-------|-------------------|-------|-------|
|id    |INTEGER|通貨情報のID        |◯      |◯      |
|type  |STRING |通貨の種類          |       |◯      |
|price |INTEGER|通貨の価格          |       |◯      |
|date  |DATE   |通貨情報を取得した日付|       |◯      |

- Tweetテーブル

|カラム  |型     |内容         |主キー|NOT NULL|
|-------|-------|------------|-----|--------|
|id     |INTEGER|ツイートのID |◯     |◯        |
|content|STRING |ツイート本文  |     |◯       |
|date   |DATE   |ツイート取得日|     |◯        |

- Articleテーブル

|カラム  |型     |内容       |主キー |NOT NULL|
|-------|-------|----------|------|---------|
|id     |INTEGER|記事のID   |◯     |◯       |
|content|STRING |記事本文   |      |         |
|date   |DATE   |記事取得日  |      |        |

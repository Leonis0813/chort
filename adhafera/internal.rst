設計仕様
========

設計仕様では以下を定義する

- :ref:`adh-int-class`
- :ref:`adh-int-sequence`

.. _adh-int-class:

モジュール構成
--------------

*クラス図*


.. uml::

   class MainActivity {
     + {static} INPUT_SIZE : int = 5
     - {static} LOADER_ID : int = 0
     + registPayment(inputs : String[]) : void
     + noticeError(errorMessage : String, ids : ArrayList) : void
     + settle() : void
   }

   class RegistrationView {
     - {static} LABELS : String[] = {"日付", "内容", "カテゴリ", "金額"}
     - fields : EditText[]
     - errorCheckers : TextView[]
     - radioGroup : RadioGroup
     - OK : Button
     - cancel : Button
     - settleView : TextView
     - context : Context
     + RegistrationView(context : Context, attributeSet : AttributeSet)
     + setToday() : void
     + showMessage(message : String) : void
     + showWrongInput(ids : ArrayList) : void
     + showSettlement(settlement : String) : void
     + resetField() : void
     + onClick(v : View) : void
   }

   class InputChecker {
     - datePattern : Pattern
     - pricePattern : Pattern
     + InputChecker()
     + checkEmpty(inputs : String[]) : ArrayList
     + checkDate(date : String) : boolean
     + checkPrice(price : String) : boolean
   }

   class HTTPClient {
     - {static} host : String
     - port : String
     - con : HttpUrlConnection
     - param : JSONObject
     - response : HashMap
     + HTTPClient(context : Context, inputs : String[])
     + HTTPClient(context : Context)
     + sendRequest() : HashMap
   }

   MainActivity "1" --> "1" RegistrationView
   MainActivity "1" --> "1" InputChecker
   MainActivity "1" --> "1" HTTPClient


- MVCモデルを利用する

- Model

  - データベースに登録するため本モジュールにはモデルは存在しない

- View

  - RegistrationView

    - MainActivityから渡される情報を表示するクラス

- Controller

  - MainActivity

    - 画面からの情報やデータベースから取得したデータを処理するクラス

- InputChecker

  - 正しく入力されているかをチェックするクラス
  - 全項目が入力されているかをチェックする
  - 日付，金額が正しく入力されているかをチェックする

- HTTPClient

  - データベースサーバへアクセスし，以下を行うクラス

    - 収支情報の登録
    - 収支の取得

.. _adh-int-sequence:

処理手順
--------

- `収支を登録する <http://localhost/adhafera_docs/design_spec.html#id4>`__
- `今月の収支を確認する <http://localhost/adhafera_docs/design_spec.html#id5>`__

収支を登録する
^^^^^^^^^^^^^^

.. uml::

   autonumber

   actor 利用者
   利用者 -> RegistrationView : onClick
   RegistrationView -> MainActivity : registPayment
   MainActivity -> InputChecker : checkEmpty

   autonumber stop
   InputChecker --> MainActivity

   alt 空欄がある
     MainActivity -> RegistrationView : showMessage
     RegistrationView --> MainActivity
     MainActivity -> RegistrationView : showWrongInput
     RegistrationView --> 利用者
   end

   autonumber resume
   MainActivity -> InputChecker : checkDate

   autonumber stop
   InputChecker --> MainActivity

   alt 空欄がある
     MainActivity -> RegistrationView : showMessage
     RegistrationView --> MainActivity
     MainActivity -> RegistrationView : showWrongInput
     RegistrationView --> 利用者
   end

   autonumber resume
   MainActivity -> InputChecker : checkPrice

   autonumber stop
   InputChecker --> MainActivity

   alt 空欄がある
     MainActivity -> RegistrationView : showMessage
     RegistrationView --> MainActivity
     MainActivity -> RegistrationView : showWrongInput
     RegistrationView --> 利用者
   end

   autonumber resume
   MainActivity -> HTTPClient : sendRequest

   autonumber stop
   HTTPClient --> MainActivity

   autonumber resume
   MainActivity -> RegistrationView : showMessage

   autonumber stop
   RegistrationView --> MainActivity

   autonumber resume
   MainActivity -> RegistrationView : resetField

   autonumber stop
   RegistrationView --> MainActivity

   autonumber resume
   MainActivity -> RegistrationView : setToday

   autonumber stop
   RegistrationView --> MainActivity

   autonumber resume
   MainActivity -> MainActivity : settle
   MainActivity -> HTTPClient : sendRequest

   autonumber stop
   HTTPClient --> MainActivity

   autonumber resume
   MainActivity -> RegistrationView : showSettlement

   autonumber stop
   RegistrationView --> 利用者

1. 利用者が収支情報を入力して登録ボタンを押すと，onClickメソッドが実行される
2. registPaymentメソッドを実行して受け取った収支情報を処理する
3. checkEmptyメソッドで空欄のチェックを行う
4. checkDateメソッドで日付のフォーマットのチェックを行う
5. checkPriceメソッドで金額のチェックを行う
6. 入力情報に問題が無ければ，sendPaymentメソッドで収支情報を送信する
7. 送信結果が返ると，noticeResultメソッドで結果を表示する
8. showMessageメソッドで登録結果を利用者に通知する
9. 入力欄を空にする
10. 利用日を入力する
11. settleメソッドを実行して収支情報を取得する
12. showSettlementメソッドを実行して収支を画面に表示する

今月の収支を確認する
^^^^^^^^^^^^^^^^^^^^

.. uml::

   autonumber
   actor 利用者
   利用者 -> MainActivity : onCreate
   MainActivity -> MainActivity : settle
   MainActivity -> HTTPClient : sendRequest
   autonumber stop
   HTTPClient --> MainActivity
   autonumber resume
   MainActivity -> RegistrationView : showSettlement
   autonumber stop
   RegistrationView --> 利用者

1. 利用者がアプリを起動すると，settleメソッドが実行される
2. sendRequestメソッドを実行してデータベースサーバから収支情報を取得する
3. showSettlementメソッドを実行して画面に今月の収支を表示する

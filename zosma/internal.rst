詳細仕様
========

設計仕様では以下を定義する

- :ref:`int-class`
- :ref:`int-sequence`

.. _int-class:

モジュール構成
--------------

*クラス図*

.. uml::

   class ApplicationController {
     + searchPayments(conditions : String[]) : void
   }

   class ViewController {
     - layout : GridBagLayout
     - columnNames : String[]
     - {static} FRAME_WIDTH : int = 1440
     - {static} FRAME_HEIGHT : int = 760
     - {static} TABLE_WIDTH : int = 700
     - {static} TABLE_HEIGHT : int = 660
     - createConstraints(x : int, y : int, width : int, height : int) : GridBagLayout
     + resetMessageField() : void
     + noticeInputError(ids : ArrayList) : void
     + resetTables() : void
     + showPayments(paymentList : ArrayList) : void
   }

   abstract class PaymentTable {
     - tableModel : DefaultTableModel
     - sorter : TableRowSorter
     + {static} columnNames : String[]
     + {static} columnWidths : int[]
     + {static} columnClasses : Class[]
     + {static} DATE : int = 0
     + {static} CONTENT : int = 1
     + {static} CATEGORY : int = 2
     + {static} PRICE : int = 3
     + {static} PAYMENT_TYPE : int = 4
     + addPayment(payment : String[]) : void
   }

   class IncomeTable
   class ExpenseTable

   class SearchForm {
     - ctrl : ApplicationController
     - {static} fieldSizes : int[]
     - labels : JLabel[]
     - checkers : JLabel[]
     - searchButton : JButton
     + showWrongInput(ids : ArrayList) : void
     + actionPerformed() : void
   }

   class MessageField {
     - textField : JTextField
     + showMessage(message : String) : void
   }

   class DateForm {
     - dateBefore : JTextField
     - dateAfter : JTextField
     + getDateBefore() : String
     + getDateAfter() : String
   }

   class ContentForm {
     - content : JTextField
     - contentType : JComboBox
     + getContent() : String
     + getContentKey() : String
   }

   class CategoryForm {
     - category : JTextField
     + getCategory() : String
   }

   class PriceForm {
     - priceUpper : JTextField
     - priceLower : JTextField
     + getPriceUpper() : String
     + getPriceLower() : String
   }

   class InputChecker {
     - datePattern : Pattern
     - pricePattern : Pattern
     - dateFormat : DateFormat
     + checkDate(date : String) : boolean
     + checkPrice(price : String) : boolean
   }

   class HTTPClient {
     - con : HttpUrlConnection
     - url : URL
     - {static} host : String
     - port : String
     - path : String
     - query : String
     - response : ArrayList
     + getPayments(condition : HashMap) : ArrayList
   }

   ApplicationController "1" -left-> "1" InputChecker
   ApplicationController "1" -right-> "1" HTTPClient
   ApplicationController "1" -down-> "1" ViewController
   ViewController "1" -left-> "2" PaymentTable
   ViewController "1" -down-> "1" SearchForm
   ViewController "1" -right-> "1" MessageField
   PaymentTable <|-- IncomeTable
   PaymentTable <|-- ExpenseTable
   SearchForm "1" *-- "1" DateForm
   SearchForm "1" *-- "1" ContentForm
   SearchForm "1" *-- "1" CategoryForm
   SearchForm "1" *-- "1" PriceForm

- MVCモデルを利用する

  - Model

    - データベースに登録されている情報を表示するだけのモジュールであるためModelは存在しない

  - View

    - PaymentTable

      - 取得した収支を表示するテーブル

    - SerachForm

      - 検索フォームを表すクラス

        - DateForm

          - 日付に関する条件を入力するフォーム
          - dateBefore: 指定した日付以前の収支を検索する
          - dateAfter: 指定した日付以降の収支を検索する
          - dateBefore, dateAfterとも指定した日付は検索対象に含まれる

        - ContentForm

          - 内容に関する条件を入力するフォーム
          - content: 指定した内容の収支を検索する
          - contentType: 全文一致か部分一致かを指定する

        - CategoryForm

          - カテゴリに関する条件を入力するフォーム
          - category: 指定したカテゴリに一致する収支を検索する

        - PriceForm

          - 金額に関する条件を入力するフォーム
          - priceUpper: 指定した金額以上の収支を検索する
          - priceLower: 指定した金額以下の収支を検索する
          - priceUpper, priceLowerとも指定した金額は検索対象に含まれる

    - MessageField

      - エラー等を表示するテキストフィールド

  - Controller

    - ApplicationController

      - アプリ全体を管理するコントローラー

    - ViewController

      - ビューの各コンポーネントを管理するコントローラー

  - InputChecker

    - 入力された条件のフォーマットが正しいかどうかをチェックするクラス

  - HTTPClient

    - DBサーバにリクエストを送信するクラス

.. _int-sequence:

処理手順
--------

- :ref:`int-sequence-index`

.. _int-sequence-index:

収支を検索する
^^^^^^^^^^^^^^

*シーケンス図*

.. uml::

   autonumber

   actor 利用者
   利用者 -> SearchForm : actionPerformed
   SearchForm -> ApplicationController : searchPayments
   ApplicationController -> InputChecker : checkDate

   autonumber stop
   InputChecker --> ApplicationController

   autonumber resume
   ApplicationController -> InputChecker : checkDate

   autonumber stop
   InputChecker --> ApplicationController

   autonumber resume
   ApplicationController -> InputChecker : checkPrice

   autonumber stop
   InputChecker --> ApplicationController

   autonumber resume
   ApplicationController -> InputChecker : checkPrice

   autonumber stop
   InputChecker --> ApplicationController

   alt 入力が不正
     ApplicationController -> ViewController : noticeInputError
     ViewController -> MessageField : showMessage
     MessageField --> ViewController
     ViewController -> SearchForm : showWrongInput
     SearchForm --> 利用者
   end

   autonumber resume
   ApplicationController -> ViewController : resetMessageField
   ViewController -> MessageField : showMessage

   autonumber stop
   MessageField --> ViewController
   ViewController --> ApplicationController

   autonumber resume
   ApplicationController -> HTTPClient : getPayments

   autonumber stop
   HTTPClient --> ApplicationController

   autonumber resume
   ApplicationController -> ViewController : resetTables

   autonumber stop
   ViewController --> ApplicationController

   autonumber resume
   ApplicationController -> ViewController : showPayments
   ViewController -> PaymentTable : addPayment

   autonumber stop
   PaymentTable --> ViewController

   autonumber resume
   ViewController -> MessageField : showMessage

   autonumber stop
   MessageField --> ViewController
   ViewController --> ApplicationController
   ApplicationController --> SearchForm
   SearchForm --> 利用者

- 利用者が検索画面を開いてから収支を表示するまでの流れ

  1. 利用者が検索条件を入力して登録ボタンを押すと，actionPerformedメソッドが実行される
  2. 入力された条件を引数にして，searchPaymentsメソッドが実行される
  3. checkDateメソッドでDateFormのdateBeforeに格納されている値のフォーマットのチェックを行う
  4. checkDateメソッドでDateFormのdateAfterに格納されている値のフォーマットのチェックを行う
  5. checkPriceメソッドでPriceFormのpriceUpperに格納されている値のチェックを行う
  6. checkPriceメソッドでPriceFormのpriceLowerに格納されている値のチェックを行う
  7. 不正な条件があればMessageFieldにエラーを通知する文字列をセットする
  8. さらに，SearchForm内の不正な入力があった項目に対応するラベルにチェックマークをセットする
  9. 入力された条件に問題が無ければ，getPaymentsメソッドを実行してリクエストを送信する
  10. 取得した収支をテーブルに表示する
  11. 取得した収支の数をMessageFieldに表示する

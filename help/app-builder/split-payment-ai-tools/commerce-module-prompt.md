---
title: '分割支払いPOC: Commerce モジュール AI プロンプト'
description: このプロンプトを使用してClient_SplitPaymentを生成する方法を説明します。 REST、プラグイン、チェックアウト、JavaScript、I/O イベント、有効にする、コンパイル、デプロイの各コマンド。
feature: App Builder, Backend Development, Eventing, Extensibility, Paas, REST, Orders
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 503
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 8dfbf2694378aae76c91afa11bfee7d93077d8ba
workflow-type: tm+mt
source-wordcount: '1207'
ht-degree: 1%

---

# 分割支払いPOC: Commerce モジュール AI プロンプト

このページを使用して、分割支払証明のコンセプト用の`Client_SplitPayment`処理中モジュール（REST、セッション処理、**[!UICONTROL Checkout]**、および&#x200B;**[!UICONTROL Admin]** ディスプレイ）を生成するプロンプト全体をコピーします。 オペレーターワークフローはApp Builderに残ります。

## このプロンプトの使用方法

**プロンプト開始**&#x200B;から&#x200B;**プロンプト終了**&#x200B;までのすべての項目をカーソル（Claudeを含む）または直接Claudeにコピーします。 Commerce プロジェクトのルートまたはAIがファイルを作成できるディレクトリから実行します。

## 実行前にカスタマイズ

* `Client`を実際のベンダー名に置き換えます。
* 別のモジュール名が必要な場合は、`SplitPayment`を変更します。
* サイトでカスタムテーマを使用している場合は、レイアウト XMLとRequireJSのパスを変更する必要があります。
* **[!UICONTROL Cash on delivery]** メソッドで`cashondelivery`とは異なるコードを使用している場合は、`payment-method-helper.js`を更新してください。


## プロンプト

**プロンプト開始**

分割支払い機能用に、実稼動対応のAdobe Commerce 2.4.5以降のプロセス内モジュールを作成します。 このモジュールは、適切なREST サーフェスを公開し、Commerce ライフサイクルの適切なタイミングで適切なデータを添付するシン PHP アダプタです。 すべてのオペレーターワークフローロジックはAdobe App Builderにあります（このモジュールにはありません）。

**モジュール ID:**
* ベンダー：`Client`
* モジュール：`SplitPayment`
* 氏名：`Client_SplitPayment`
* 名前空間：`Client\SplitPayment`
* 場所：`app/code/Client/SplitPayment/`
* 依存関係：`Magento_Checkout`、`Magento_CustomerBalance`、`Magento_Sales`、`Magento_Quote`、`Magento_WebApi`、`Magento_AdobeCommerceEventsClient`

以下のファイル構造にリストされているすべてのファイルを生成します。 ファイルを省略しないでください。 すべてのPHP ファイルで`declare(strict_types=1)`を使用します。


### 生成するファイル構造

```
app/code/Client/SplitPayment/
├── registration.php
├── composer.json
├── Api/
│   ├── Data/SplitPaymentInterface.php
│   └── SplitPaymentManagementInterface.php
├── Block/
│   └── Order/SplitPaymentInfo.php
├── Controller/
│   └── Checkout/StoreCreditBalance.php
├── etc/
│   ├── acl.xml
│   ├── config.xml
│   ├── db_schema.xml
│   ├── db_schema_whitelist.json
│   ├── di.xml
│   ├── events.xml
│   ├── extension_attributes.xml
│   ├── io_events.xml
│   ├── module.xml
│   ├── webapi.xml
│   └── frontend/
│       └── routes.xml
├── Model/
│   ├── SplitPaymentData.php
│   ├── SplitPaymentManagement.php
│   ├── Service/
│   │   └── SplitInvoiceService.php
│   └── Session/
│       └── SplitPaymentSession.php
├── Observer/
│   ├── AutoInvoiceStoreCreditOnOrderPlace.php
│   └── CopySplitPaymentToOrder.php
├── Plugin/
│   ├── CheckoutPlugin.php
│   ├── OrderRepositoryPlugin.php
│   ├── PlaceOrderPlugin.php
│   ├── Adminhtml/
│   │   └── OrderPaymentPlugin.php
│   ├── Checkout/
│   │   └── LayoutProcessorPlugin.php
│   ├── CustomerBalance/
│   │   └── CapCustomerBalanceCollectPlugin.php
│   ├── Payment/
│   │   ├── Block/
│   │   │   └── AdminSplitPaymentTitlePlugin.php
│   │   └── Checks/
│   │       └── SplitPaymentZeroTotalPlugin.php
│   ├── Quote/
│   │   └── FixSplitPaymentGrandTotalPlugin.php
│   └── Sales/
│       └── FixInvoiceCustomerBalanceAfterTotalsPlugin.php
├── Setup/
│   └── Patch/
│       └── Data/
│           └── AddSplitPaymentOrderAttribute.php
└── view/
    └── frontend/
        ├── requirejs-config.js
        ├── layout/
        │   └── sales_order_view.xml
        ├── templates/
        │   └── order/
        │       └── split-payment-info.phtml
        └── web/
            ├── js/
            │   ├── model/
            │   │   └── payment-method-helper.js
            │   └── view/
            │       └── payment/
            │           ├── cashondelivery-method.js
            │           └── split-payment.js
            └── template/
                └── payment/
                    ├── cashondelivery.html
                    └── split-payment.html
```


### 行動の仕様

#### &#x200B;1. データベーススキーマ （`etc/db_schema.xml`）

これらの列を`sales_order`に追加します（リソース：`sales`）:

| 列 | タイプ | 無効化できる | コメント |
|---|---|---|---|
| `split_store_credit_amount` | decimal （12,4） | はい | クレジットパートを保存 |
| `split_cash_amount` | decimal （12,4） | はい | 現金部分 |
| `split_cash_status` | varchar （32） | はい | `pending` / `received` / `declined` |
| `split_sc_invoice_id` | int unsigned | はい | クレジット請求書エンティティ IDの保存 |
| `split_cash_invoice_id` | int unsigned | はい | 現金請求書エンティティ ID |

これらの列の`db_schema_whitelist.json`も生成します。

#### &#x200B;2. 拡張機能の属性（`etc/extension_attributes.xml`）

`split_store_credit_amount` （浮動小数）、`split_cash_amount` （浮動小数）、`split_cash_status` （文字列）を次の場所に追加：
* `Magento\Quote\Api\Data\CartInterface`
* `Magento\Sales\Api\Data\OrderInterface`
* `Magento\Sales\Api\Data\OrderPaymentInterface`

#### &#x200B;3. REST エンドポイント （`etc/webapi.xml`）

```xml
POST /V1/split-payment/set              → anonymous (session-scoped)
POST /V1/split-payment/orders/:orderId/cash-received  → Magento_Sales::actions
POST /V1/split-payment/orders/:orderId/cash-decline   → Magento_Sales::cancel
```

3つすべてが`Client\SplitPayment\Api\SplitPaymentManagementInterface`にマップされます。

#### &#x200B;4. I/O イベント （`etc/io_events.xml`）

`observer.sales_order_place_before`を購読してフィールドを含める：
`entity_id`, `quote_id`, `increment_id`, `subtotal`, `split_store_credit_amount`, `split_cash_amount`, `split_cash_status`

#### &#x200B;5. `SplitPaymentSession` — セッションラッパー

分割が宣言された金額をチェックアウトセッションに格納します。 公開する必要があります：
* `setAmounts(float $storeCredit, float $cash): void`
* `getAmounts(): array` — `['store_credit' => float, 'cash' => float]`を返します
* `clear(): void`
* `beginBalanceApply(): void` — ストア クレジットの適用中に総修正プラグインを抑制するフラグを設定します
* `endBalanceApply(): void`
* `isBalanceApplyInProgress(): bool`

#### &#x200B;6. `SplitPaymentManagement` — REST コントローラー

**`setSplitPayment(float $storeCreditAmount, float $cashAmount, ?string $cartId = null): bool`**
* カートが現在のセッションに属していることを検証します（数値とマスクされた見積もりIDを比較して）
* 金額を`SplitPaymentSession`に格納します
* 成功すると`true`を返します。失敗すると、汎用メッセージで`LocalizedException`をスローします

**`markCashReceived(int $orderId): bool`**
* `entity_id`様の注文を読み込みます
* `split_cash_status === 'pending'`を検証します
* ステータスを`received`に、状態を`processing`に設定します
* 履歴コメントを追加します：`"Cash payment of $X.XX received."`
* `SplitInvoiceService::createCashPortionInvoice($orderId)`を呼び出します
* 現金請求書の増分IDを含むコメントを追加します
* `createShipmentIfPossible($orderId)`を呼び出す – 出荷可能なアイテムがある場合は、`ShipOrder::execute()`を使用して出荷を作成します
* `true`を返します。任意のエラーで汎用`LocalizedException`をスローします

**`markCashDeclined(int $orderId): bool`**
* 注文を読み込む
* `split_cash_status === 'pending'`を検証します
* `$order->canCancel()`を検証します
* ステータスを`declined`に設定し、コメントを追加：`"Cash payment declined (simulated fraud check)."`
* 注文を保存
* `OrderManagement::cancel($orderId)`を呼び出します
* `true`を返します。失敗すると汎用`LocalizedException`がスローされます

#### &#x200B;7. `PlaceOrderPlugin` — `QuoteManagement`のaroundPlaceOrder

これは最も重要なプラグインです。 `aroundPlaceOrder`を実行：

1. 見積もりを読み込みます。アクティブでない場合は、すぐに`$proceed()`に電話してください
2. `SplitPaymentSession::getAmounts()`を読む
3. 引用符に`customer_balance_amount_used > 0`が付いている場合は、`BalanceManagementInterface::remove($cartId)`を呼び出します（`LocalizedException`を処理 – ログに記録し、汎用メッセージとして再送信）
4. `recollectQuoteForBalanceOperation()`を呼び出し – 見積もりを読み込み、`collectTotals()`を呼び出し、保存します（`beginBalanceApply()` / `endBalanceApply()`以内に収まると、総修正プラグインが表示されません）
5. `$storeCredit > 0`の場合は、`BalanceManagementInterface::apply($cartId, $storeCredit)`に電話してください（失敗を処理）
6. 見積もりを再読み込みします。拡張機能の属性を設定：`split_store_credit_amount`、`split_cash_amount`、`split_cash_status = 'pending'`
7. `payment->additional_information['client_split_payment']`を`['store_credit' => x, 'cash' => y]`として保存
8. 見積もりを保存
9. `$proceed()`への通話、注文IDの取得
10. `SplitPaymentSession::clear()`に電話
11. 返品注文ID

#### &#x200B;8. `CopySplitPaymentToOrder` — `sales_model_service_quote_submit_before`の監視者

セッション→支払いadditional_information→見積もり拡張機能の属性（優先順位付け）から分割金額を読み取ります。 `split_store_credit_amount`、`split_cash_amount`、`split_cash_status = 'pending'`を注文オブジェクトに書き込みます。

#### &#x200B;9. `AutoInvoiceStoreCreditOnOrderPlace` — `sales_order_place_after`の観察者

注文後、注文に店舗のクレジット金額（`split_store_credit_amount > 0`）がある場合は、`SplitInvoiceService::createStoreCreditPortionInvoice($orderId)`に電話して、店舗のクレジット部分をすぐに請求します。

#### 10. `SplitInvoiceService`

**`createStoreCreditPortionInvoice(int $orderId): ?int`**
店舗のクレジット部分に対してのみ請求書を作成します。 請求書の`customer_balance_amount`を店舗のクレジット金額に設定します。 請求書を登録して保存します。 失敗した場合に請求書エンティティ IDまたはnullを返します（ログ、再投しないでください）。

**`createCashPortionInvoice(int $orderId): ?int`**
現金部分（SC請求書でカバーされていない残りの注文品目/金額）の請求書を作成します。 登録と保存。 失敗した場合にエンティティ IDまたはnullを返します。

#### &#x200B;11. `CheckoutPlugin` — `PaymentInformationManagement`

`Magento\Checkout\Model\PaymentInformationManagement::savePaymentInformationAndPlaceOrder`のプラグイン。 次に進む前に：
* チェックアウトセッションから見積もりを読み込みます
* 設定されたしきい値を取得：`Magento\Framework\App\Config\ScopeConfigInterface::getValue('split_payment/general/threshold')`、デフォルト：`100`
* `$quote->getGrandTotal() > $threshold`の場合、`LocalizedException('Payment could not be processed. Please try again or contact support.')`をスローします

#### &#x200B;12. `CapCustomerBalanceCollectPlugin` — `Customerbalance`合計

ネイティブ顧客バランス合計の収集が実行された後、`customer_balance_amount_used`から`SplitPaymentSession::getAmounts()['store_credit']`までの上限を設定します。 これにより、お客様が店舗のクレジット部分を小さく宣言した場合に、Commerceがお客様の残高を過大に適用するのを防ぐことができます。

#### &#x200B;13. `FixSplitPaymentGrandTotalPlugin` — `Quote\Address\Total\Grand`

総計の収集後：分割支払セッションが存在し、`isBalanceApplyInProgress()`がfalseの場合、見積もり総計をセッションの現金金額に設定します。 これにより、チェックアウト UIに現金の支払い期限のみが表示されます。

#### &#x200B;14. `FixInvoiceCustomerBalanceAfterTotalsPlugin` — `Sales\Model\Order\Invoice`

請求書の合計を収集した後、請求書の関連する注文に`split_sc_invoice_id`がある場合は、請求書の`customer_balance_amount`を修正して、店舗クレジットの二重適用を防ぎます。

#### &#x200B;15. `SplitPaymentZeroTotalPlugin` — `Payment\Model\Checks\ZeroTotal`

見積総額が$0であっても、`SplitPaymentSession::getAmounts()['cash'] > 0`の場合にCOD支払いを許可します（店舗のクレジットは注文全体をカバーするため）。

#### &#x200B;16. `LayoutProcessorPlugin` — `Checkout\Block\Checkout\LayoutProcessor`

レイアウトが処理された後：
* `Client_SplitPayment/js/view/payment/split-payment` コンポーネントを`components.checkout.children.steps.children.billing-step.children.payment.children.renders.children.offline-payments.children.cashondelivery.children.additional`の`cashondelivery`支払い方法コンポーネントの`additional`子に挿入します
* ネイティブストアクレジット UI コンポーネント （`customerBalance`, `useStoreCredit`）を支払い手順から削除します。分割された支払いコンポーネントがストアクレジットの表示/アプリケーションを所有します

#### &#x200B;17. `OrderRepositoryPlugin` — `OrderRepositoryInterface`

`get()`と`getList()`の後、フラット `sales_order`列（`split_store_credit_amount`、`split_cash_amount`、`split_cash_status`）から注文の拡張属性をハイドレートします。

#### &#x200B;18. `AdminSplitPaymentTitlePlugin` — `Payment\Block\Info`

`getTitle()`が返った後、支払い方法が`cashondelivery`で、注文に分割支払いがある場合は、タイトルに`" (Split: Cash $X.XX + Store Credit $Y.YY)"`を追加します。

#### &#x200B;19. `OrderPaymentPlugin` — `Sales\Block\Adminhtml\Order\Payment`

`_beforeToHtml`またはafterToHtml経由で、Commerce管理者注文ビューのHTMLの支払いブロックに分割支払いの詳細（現金金額、ストアクレジット金額、ステータス）を追加します。

#### &#x200B;20. ストアフロント店舗クレジット残高コントローラー

`Controller/Checkout/StoreCreditBalance.php` — ルート：`GET /splitpayment/checkout/storecreditbalance`

JSON: `{"balance": float, "logged_in": bool}`を返します。 お客様がログインしていない場合は、`{"balance": 0, "logged_in": false}`を返します。 `Magento\CustomerBalance\Model\Balance`から残高を読み取ります。

`etc/frontend/routes.xml`のフロントエンドルートを`frontName="splitpayment"`に登録します。

#### &#x200B;21. チェックアウト KnockoutJS コンポーネント — `split-payment.js`

次の条件を満たす`uiComponent`:
* `cashondelivery`の支払い方法が選択されたときに検出します（`quote.paymentMethod.subscribe`）
* `GET /splitpayment/checkout/storecreditbalance`経由で顧客の店舗クレジット残高を読み込みます
* 現金金額フィールドに全注文額を事前入力します（`grand_total`および`customerbalance`を除く`total_segments`から計算）。現金残余に設定される可能性があるため、`grand_total`を直接使用しません）
* お客様が現金金額の入力を変更すると、必要なストアクレジット =注文合計−現金が計算されます。検証；`POST /V1/split-payment/set`への投稿
* 現金/注文合計、不十分な店舗クレジット、ログインしていない場合の検証メッセージを表示します
* 有効な分割が入力されたときに成功メッセージを表示します：`"The remaining $X.XX will automatically be applied from your store credit."`
* 別の支払い方法が選択されている場合にリセットします（`{storeCreditAmount: 0, cashAmount: 0}`を投稿してセッションをクリア）

#### 22. `cashondelivery-method.js`

`Magento_OfflinePayments/js/view/payment/offline-payments`を拡張します。 `payment-method-helper.js`を使用して現金方法コードを検出します。 `split-payment` コンポーネントを`additional`地域に登録します。

#### 23. `payment-method-helper.js`

`getCashMethodCode()`を返すユーティリティ — `cashondelivery`について`window.checkoutConfig.paymentMethods`を確認します。必要に応じて`checkmo`にフォールバックします。

#### &#x200B;24. `cashondelivery.html` テンプレート

標準COD テンプレートですが、`<!-- ko foreach: getRegion('additional') -->`地域が含まれているため、分割支払い子コンポーネントをレンダリングできます。

#### &#x200B;25. `split-payment.html` テンプレート

分割支払いフィールドのKnockoutJS テンプレート：
* 使用可能な店舗のクレジット残高の表示
* 現金金額入力（数値、手順0.01）
* ストアのクレジット部分の表示（読み取り専用）
* ストアクレジットメッセージを自動適用（分割が有効で、ストアクレジットが0を超える場合に表示）
* 検証エラーメッセージ

#### 26. `requirejs-config.js`

マップ：
* コンポーネント→`Client_SplitPayment/js/view/payment/split-payment`
* `Client_SplitPayment/js/view/payment/cashondelivery-method`→CODの上書き
* ヘルパー→`Client_SplitPayment/js/model/payment-method-helper`

#### 27. `etc/config.xml`

デフォルトのシステム設定値：

```xml
<split_payment>
  <general>
    <threshold>100</threshold>
    <enabled>1</enabled>
  </general>
</split_payment>
```


### 重要な実装に関するメモ

**ストアクレジットアプリケーションでは、直接モデル操作ではなく`BalanceManagementInterface`を使用する必要があります。** `BalanceManagementInterface::apply()`は、セッション、検証、および買い物かごの再計算をアトミックに処理します。

**`PlaceOrderPlugin`は`aroundPlaceOrder`を使用する必要があります（`beforePlaceOrder`ではありません）。** 買い物かごがまだアクティブな間は、ストアクレジットを適用する必要があります。これは、`$proceed()`が呼び出される前に保証する必要があります。

**`beginBalanceApply` / `endBalanceApply`のセッションフラグパターンは重要です。** これを指定しない場合、`FixSplitPaymentGrandTotalPlugin`は残高処理中`collectTotals()`に実行され、総計が現金残余に設定され、`BalanceManagementInterface::apply()`が失敗するか、クレジットに上限が設定されます。

**内部エラーの詳細をお客様に公開しないでください。** REST応答に表示されるすべての`catch` ブロックは`LocalizedException('Payment could not be processed. Please try again or contact support.')`をスローする必要があります。

**`entity_id`は数値データベース IDです。** APP BUILDERからのREST呼び出しは、常に`increment_id`ではなく`entity_id`を使用します。

**`SplitInvoiceService`は、エラーを伝搬するのではなく、エラーを検出してログに記録する必要があります。** 請求書作成エラーは、既に配置された注文をキャンセルしないでください。エラーをログに記録し、管理者が手動で処理できるようにします。


### ファイルの生成後

Commerce プロジェクトルートで次のコマンドを実行します。

```bash
bin/magento module:enable Client_SplitPayment
bin/magento setup:upgrade
bin/magento setup:di:compile
bin/magento setup:static-content:deploy -f
bin/magento cache:flush
```

### プロンプトの終了


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

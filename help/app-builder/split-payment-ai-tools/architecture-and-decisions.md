---
title: 分割払いPOC：アーキテクチャとデザインの決定
description: 分割支払いPOCが、拡張機能の属性、REST、5つのプラグインエッジケースを使用して、チェックアウトをCommerceに同期し、I/O駆動型のステップをApp Builderにマッピングする方法について説明します。
feature: App Builder, Eventing, Extensibility, Paas, Payments, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 293
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 9add0b4bfa1eba33ec359adaa766b64711df25ba
workflow-type: tm+mt
source-wordcount: '863'
ht-degree: 1%

---

# 分割払いPOC：アーキテクチャとデザインの決定

このページでは、分割支払い概念実証の背後にあるアーキテクチャの選択について説明します。 このシリーズでは、ビルドプロンプトを使用する前に、各コンポーネントの構造と、独自のプロジェクト内のパターンの適応方法について理解しておきましょう。

## 基本原則

概念実証は、最も洗練された分割支払いの実装についてではありません。 ビッグバンの書き換えなしで&#x200B;**Commerce ロジックをApp Builderに移行する方法を示します**。

全体を通して適用されるルールは次のとおりです。

> **何かがCommerce リクエストサイクルで同期的に実行される必要がある場合、またはクリーンな外部サーフェスを持たないCommerce内部APIを呼び出す必要がある場合、PHP内に残ります。 それ以外はすべてApp Builderに移動します。**

## Commerce（PHP）の概要とその理由

### &#x200B;1. ストア クレジット アプリケーション：`PlaceOrderPlugin`

店舗のクレジットは、`Magento\CustomerBalance\Api\BalanceManagementInterface::apply()`を使用してカートに適用されます。 この方法は、**アクティブ**&#x200B;の買い物かごでのみ機能します。 注文が確定すると、カートは無効になります。 App Builderは、注文が完了した&#x200B;*後にI/O イベント*&#x200B;を受け取るので、App Builderから店舗クレジットを適用することはできません。

**レッスン :**&#x200B;注文前に買い物かごの状態を変更する必要があるすべては、Commerceで実行する必要があります。 回避策はありません。

### &#x200B;2. 同期しきい値ガード：`CheckoutPlugin`

$100の注文しきい値チェックでは、顧客が&#x200B;**[!UICONTROL Place Order]**&#x200B;を選択する前に、支払い手順で顧客をブロックする必要があります。 応答は、Commerce リクエストサイクルで同期している必要があります。 App Builderはイベント駆動型で非同期なので、その瞬間に直ちにエラーを返すことはできません。

App Builder *も*&#x200B;は（監査として）しきい値を検証しますが、カスタマーエクスペリエンスは、最初に実行されるCommerce チェックによって異なります。

### &#x200B;3. カスタム REST エンドポイント：`webapi.xml`および`SplitPaymentManagement`

以下のエンドポイントが必要です。

* `SplitInvoiceService`を呼び出します（Commerce内部請求書サービスを使用する請求書）
* `ShipOrder::execute()`を呼び出します（Commerceの社内配送サービス）
* Commerceの注文状況マシンで注文状況とステータスを更新する

`/V1/split-payment/orders/:id/cash-received`と`/V1/split-payment/orders/:id/cash-decline`

このビヘイビアーにはクリーンなパブリック REST レイヤーがないので、Commerceはエンドポイントを公開します。 App Builderが呼び出します。

### &#x200B;4. 見積もりと注文の金額を分割：オブザーバーとプラグイン

Commerceでは、App Builderが読み取るREST応答と&#x200B;**[!UICONTROL Admin]**&#x200B;注文ビューの両方で、注文に分割金額（`split_store_credit_amount`、`split_cash_amount`、`split_cash_status`）が必要です。 金額は拡張子属性で添付され、引用符から`sales_model_service_quote_submit_before`の観察者の順序にコピーされます。

## App Builderの概要とその理由

### &#x200B;1. イベント駆動型の注文処理：`payment-orchestrator`

`sales_order_place_before`が発生すると、App Builderはイベントを受け取ります。 しきい値を再検証し（監査として）、注文に対する&#x200B;*現金保留中*&#x200B;のコメントを記録し、注文ステータスを更新します。 これには新しいPHPは必要なく、CommerceへのRESTのみが必要です。

### &#x200B;2. 現金受け入れ：`payment-accept`

ERP （またはダッシュボードのオペレーター）が現金を受け取ったことを確認すると、`payment-accept`さんが`POST /V1/split-payment/orders/:id/cash-received`に電話します。 請求書、発送、注文状況はCommerceで処理されます。 App Builderはトリガーです。

### &#x200B;3. 現金減少：`payment-decline`

`payment-decline`さんが`POST /V1/split-payment/orders/:id/cash-decline`に電話し、Commerceが注文をキャンセルします。 現金受け入れと同じパターンです。

### &#x200B;4. オペレーターダッシュボード：`demo-dashboard`

App Builder web アクションから提供される自己完結型のHTML ダッシュボード。 これは、Commerce RESTから現金を待っている注文を取得し、上記のApp Builder アクションを呼び出す&#x200B;**[!UICONTROL Accept]** / **[!UICONTROL Decline]** アクションを提供します。 Commerce **[!UICONTROL Admin]**&#x200B;は必要ありません。

## しきい値：目的に応じて2回実施

```text
Customer at checkout
        |
        v
[Commerce: CheckoutPlugin]     <- Synchronous, blocks immediately, user sees error
        |
        |  (if somehow bypassed: direct API call, and so on)
        v
[Order placed] -> I/O Event -> [App Builder: payment-orchestrator]
                                        |
                                        v
                              [evaluateThreshold()]  <- Async audit, records failure comment
```

**Commerceがユーザー向けガードを所有し、App Builderがプレースメント後の監査を所有します。** それは意図的なことです。

## ストアクレジット：なぜPHPに留まるか

```text
What you might think would work (it does not):
  Order placed -> I/O Event -> App Builder -> PUT /V1/carts/:id/store-credit
  (Fails: cart is inactive after place order)

What actually works:
  AroundPlaceOrder plugin
  -> BalanceManagementInterface::apply($cartId, $amount)  <- cart is still active
  -> place order
  -> order placed
  -> I/O event: App Builder (store credit is already applied)
```

オーケストレーターの`store-credit.js` ファイルがこれを文書化します。 なぜそれが使用されないのかを説明するコメント付きのノーアップスタブです。

## 拡張機能の属性：接着剤

分割金額は、次の拡張機能の属性でシステム内を移動します。

```text
Checkout JavaScript (Knockout)
    |  POST /V1/split-payment/set
    v
SplitPaymentSession (PHP session)
    |  AroundPlaceOrder reads the session
    v
CartInterface extension attributes
    |  `sales_model_service_quote_submit_before` observer
    v
OrderInterface extension attributes -> `sales_order` flat columns
    |  I/O event payload includes these fields
    v
App Builder `payment-orchestrator` reads the split amounts
```

## データモデル

このモジュールが追加する&#x200B;**`sales_order`個のフラット列**

| 列 | タイプ | 目的 |
| --- | --- | --- |
| `split_store_credit_amount` | 浮動小数 | 適用されたストアクレジット |
| `split_cash_amount` | 浮動小数 | 現金支払期限 |
| `split_cash_status` | varchar | `pending`、`received`または`declined` |
| `split_sc_invoice_id` | int | 店舗クレジット請求書のエンティティ ID |
| `split_cash_invoice_id` | int | 現金請求書のエンティティ ID |

**拡張機能の属性** （`CartInterface`、`OrderInterface`、`OrderPaymentInterface`上）

* `split_store_credit_amount` （浮動小数）
* `split_cash_amount` （浮動小数）
* `split_cash_status` （文字列）

## I/O イベントペイロードフィールド

`observer.sales_order_place_before`は`io_events.xml`で設定されており、イベントに次のものが含まれます。

```xml
entity_id, quote_id, increment_id, subtotal,
split_store_credit_amount, split_cash_amount, split_cash_status
```

App Builderでは、注文IDとして`entity_id`を使用し、しきい値の検証に`split_store_credit_amount`と`split_cash_amount`を使用します。

## 概念実証でカバーされている5つのエッジケース

### 1. `CapCustomerBalanceCollectPlugin`

Commerceのネイティブ **[!UICONTROL Customer balance]**&#x200B;の合計コレクターは、過剰に適用できます（セッションで宣言された分割金額ではなく、利用可能な完全な残高を確認できます）。 このプラグインは、セッションで宣言された値に金額を上限とします。

### 2. `FixSplitPaymentGrandTotalPlugin`

店舗のクレジットが適用された後、見積もり&#x200B;**[!UICONTROL Grand Total]**&#x200B;は現金のみの金額に落ちる可能性があります。 チェックアウト JavaScriptでは、その変更の&#x200B;*前*&#x200B;にスプリット検証を行うための注文合計を計算する必要があります。 プラグインは合計コレクションの後に実行され、表示を修正しますが、JavaScriptは`grand_total`のみを信頼せず、小計セグメントから値を再構築します。

### 3. `FixInvoiceCustomerBalanceAfterTotalsPlugin`

請求書の合計が再収集されると、ストアクレジットを2回適用できます。 このプラグインは、請求書の`customer_balance_amount`を修正します。

### 4. `SplitPaymentZeroTotalPlugin`

店舗のクレジットが適用された後、買い物かご&#x200B;**[!UICONTROL Grand Total]**&#x200B;は$0 （店舗のクレジット注文の全額）になります。 Commerceの&#x200B;**[!UICONTROL Zero subtotal checkout]** チェックは、その場合にCODをブロックできます。 このプラグインを使用すると、セッションのキャッシュ金額が0より大きい場合にCODを使用できます。

### &#x200B;5. `BalanceManagementInterface::apply()`以前の見積もり回収

`apply()`は、現在の&#x200B;**[!UICONTROL Grand Total]**&#x200B;に対して金額をチェックします。 合計が既に現金部分のみの場合、`apply()`は失敗または上限を設定できます。 `PlaceOrderPlugin`は、セッションフラグ （`beginBalanceApply` / `endBalanceApply`）を使用して、残高が適用されている間、総修正を一時的に一時停止します。


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

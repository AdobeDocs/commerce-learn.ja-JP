---
title: 'Split payment POC: testing and verification guide'
description: 'Learn how to verify the split payment POC: Commerce install, REST, checkout, threshold, simulation accept and decline, demo dashboard, and App Builder logs.'
feature: App Builder, Configuration, Extensibility, Paas, Payments, REST, Orders
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 359
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: beb22335cec97141b46ddbbca97d21b216c55a80
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 0%

---

# Split payment POC: testing and verification guide

This guide walks you through verifying that every component works correctly, in the order they should be tested. Start from the bottom (Commerce module) and work up (App Builder).


## Step 1 — Verify Commerce Module Installation

`bin/magento setup:upgrade`および`bin/magento setup:di:compile`の実行後：

```bash
# Confirm module is enabled
bin/magento module:status Client_SplitPayment

# Confirm db_schema columns were added
bin/magento doctrine:schema:validate
# or check directly:
mysql -u <user> -p <dbname> -e "DESCRIBE sales_order;" | grep split
```

Expected output: four columns starting with `split_` visible in `sales_order`.


## Step 2 — Verify Commerce Admin Configuration

In Commerce Admin:
1. **[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Sales]** > **[!UICONTROL Payment Methods]** — **[!UICONTROL Cash On Delivery]**&#x200B;がタイトル `Cash`で有効になっていることを確認します
2. **[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Customers]** > **[!UICONTROL Customer Configuration]** > **[!UICONTROL Store Credit Options]** – 有効化を確認
3. Confirm your test customer has store credit: **[!UICONTROL Customers]** > **[!UICONTROL All Customers]** > *[customer]* > **[!UICONTROL Store Credit]**


## Step 3 — Verify REST Endpoints Are Accessible

Use curl to confirm the endpoints respond (they will reject unauthorized requests, but a 401 confirms they&#39;re routed correctly):

```bash
# Should return 401 (not 404) — endpoint exists but requires auth
curl -X POST https://your-store.example.com/rest/V1/split-payment/orders/1/cash-received

# Should return 200 or session-based response — anonymous endpoint
curl -X POST https://your-store.example.com/rest/V1/split-payment/set \
  -H "Content-Type: application/json" \
  -d '{"storeCreditAmount": 0, "cashAmount": 0}'
```


## Step 4 — Test the Checkout UI

1. Log in to the storefront as your test customer (who has store credit)
2. 商品をカートに追加する（送料+税込み後の合計が100 ドル未満）
3. Proceed to checkout
4. At the payment step, select **Cash** (or Cash On Delivery)
5. Verify the split payment fields appear:
   * Your store credit balance shown
   * Cash amount field pre-filled with the order total
   * &quot;Store credit toward this order&quot; field showing $0.00 (since cash = full order total)
6. Reduce the cash amount (e.g., enter $10 for a $50 order)
7. ストアのクレジット部分が$40.00に更新されていることを確認します。
8. メッセージが表示されることを確認します：`"The remaining $40.00 will automatically be applied from your store credit."`

**テスト検証：**
* 注文合計を超える金額を入力する→のエラーメッセージ
* 使用可能な店舗クレジットよりも多くの店舗クレジットが必要な現金金額→入力してください
* Enter cash = 0 → error （またはストアクレジットは注文全体をカバー）


## ステップ 5 - テストしきい値ガード

1. 合計$100以上の商品を追加（小計+送料+税> $100）
2. チェックアウトに進み、**Cash**&#x200B;を選択します
3. 注文を試みます
4. エラーメッセージが表示されることを確認します：`"Payment could not be processed. Please try again or contact support."`
5. カートが保持されていることを確認します（お客様は引き続きカートを調整し、再試行できます）


## ステップ 6 - テスト分割支払い注文を行う

1. 100 ドル未満でカートを作成（店舗のクレジットを使用して顧客にログイン）
2. チェックアウト時に現金を選択
3. 注文総額より少ない金額（例：$45の注文の$10）を入力します。
4. 店舗のクレジットメッセージが表示されることを確認します
5. **注文を配置**&#x200B;をクリックします

注文後、Commerce Adminで次の項目を確認します。
* 注文のステータスは`pending_payment`
* 注文には、次の2つの履歴コメントがあります。
   1. `"Cash payment of $X.XX pending. Awaiting admin confirmation."` （App Builder `payment-orchestrator`から）
   2. `"Split payment orchestration completed. Order awaiting cash confirmation."` （App Builderから）
* 分割支払い金額は、注文支払いブロックに表示されます

> **App Builder コメントが表示されない場合：** App Builderのアクションログを`aio app logs`で確認します。 イベントが起動しなかったか、アクションにエラーがある可能性があります。


## ステップ 7 - シミュレーションスクリプトを使用したテスト受け入れ

シミュレーションスクリプトは、オペレーターUIを完全に使用せずに承認/拒否フローをテストする最も高速な方法です。

```bash
cd commerce-checkout-starter-kit
cp commerce-backend-ui-1/.env.simulation.example commerce-backend-ui-1/.env.simulation
# Edit .env.simulation with your credentials

# List recent orders (find your test order entity_id)
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs list

# Show split payment fields for a specific order
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs show 42

# Accept the cash payment
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs accept 42
```

承認したら、Commerce管理者注文ビューで確認します。
* 注文ステータスは`processing`です
* 履歴コメント：`"Cash payment of $X.XX received."`
* 作成された現金請求書（請求書タブに表示）
* 配送が作成されました（該当する場合、「出荷」タブに表示）
* 履歴コメント：`"Split payment: cash portion invoiced #XXXXXXXX."`
* 履歴コメント：`"Split payment: shipment created after cash was accepted (App Builder / API)."`


## ステップ 8 - シミュレーションスクリプトを使用したテスト拒否

別のテスト注文（ステップ 6と同じ設定）を行ってから：

```bash
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs decline <orderId>
```

辞退後、Commerce管理者で次の項目を確認します。
* 注文ステータスは`canceled`です
* 履歴コメント：`"Cash payment declined (simulated fraud check)."`
* `split_cash_status` = `declined`


## ステップ 9 - デモダッシュボードをテストする

`split-payment-orchestrator`をデプロイした後、`aio app deploy`はアクション URLを印刷します。

ブラウザーで`demo-dashboard` URLを開きます：

```
https://[runtime-host]/api/v1/web/split_payment_orchestrator/demo-dashboard
```

`DEMO_UI_SECRET`が設定されている場合：

```
https://[runtime-host]/api/v1/web/split_payment_orchestrator/demo-dashboard?secret=<your-secret>
```

保留中の注文の場合：
1. ダッシュボードには、保留中のリストに順序が表示されます
2. **承諾**&#x200B;をクリック→ると、注文はCommerceの`processing`に移動します
3. 別の注文を行います。「**辞退**」をクリック→、Commerceで`canceled`の注文を行う必要があります


## ステップ 10 - App Builder アクションログのテスト

```bash
# Follow logs in real-time
aio app logs --tail

# Or view last invocations
aio runtime activation list --limit 10
aio runtime activation logs <activation-id>
```

`payment-orchestrator`の場合、次を探します。

```
[INFO] Split payment orchestration finished { orderId: '42' }
```

`payment-accept`または`payment-decline`について：

```
[INFO] Cash payment accepted on Commerce via REST { orderId: '42' }
```


## 一般的な問題と修正

### Commerce OAuthの「署名が無効です」

**原因：** App Builder ランタイムとCommerceの間のクロックスキュー、またはOAuth署名バグ。

**修正：**
* `COMMERCE_BASE_URL`の末尾にスラッシュがないことを確認してください
* 4つのOAuth資格情報が&#x200B;_ACTIVATED_&#x200B;統合用であることを確認してください
* 最初にシミュレーションスクリプトでテストします。スクリプトが機能するが、App Builderが機能しない場合は、環境変数が読み込まれていない可能性があります（環境の変数が見つからない場合は、`aio app deploy`出力を確認してください）

### チェックアウト時に分割支払いフィールドが表示されない

**原因：** LayoutProcessorPlugin インジェクション パスがチェックアウト レイアウトと一致しません。

**修正：**
* `Client_SplitPayment/js/view/payment/split-payment`の読み込み中にRequireJS エラーが発生していないか、ブラウザーコンソールを確認してください
* `bin/magento setup:static-content:deploy`の確認が正常に完了しました
* フラッシュ キャッシュ：`bin/magento cache:flush`
* テーマにカスタムチェックアウトがある場合、コンポーネントを挿入するための`LayoutProcessorPlugin` パスを調整する必要がある場合があります

### ストアクレジットが適用されない/「支払いを処理できませんでした」プレースオーダー

**原因：**&#x200B;通常、エッジ ケース プラグインのいずれかが正しく機能しません。

**確認：**
1. `SplitPaymentSession`は正しい金額を返していますか？ `PlaceOrderPlugin`に一時的なデバッグログを追加
2. `BalanceManagementInterface::apply()`が呼び出される前に、`FixSplitPaymentGrandTotalPlugin`が実行中で見積もり合計に影響を与えていますか？ `beginBalanceApply()` フラグはそれを抑制する必要があります。
3. Commerceでカスタマーバランスモジュールが有効になっていますか？

### App Builder アクションはイベントを受け取りますが、orderIdがありません

**原因：** `io_events.xml` フィールドリストに`entity_id`が含まれていないか、イベントペイロードの形状が変更されています。

**Fix:**
* Confirm `io_events.xml` includes `entity_id` in the field list
* In the action, log `JSON.stringify(params)` temporarily to see the full payload shape
* Check that the `extractValue()` function is finding the right nesting level

### Orders don&#39;t show in demo dashboard

**Cause:** Commerce REST `orders` search criteria not returning orders, or `split_cash_status` field not in the REST response.

**Fix:**
* Confirm `OrderRepositoryPlugin` is loading extension attributes correctly
* Test directly: `GET /rest/V1/orders?searchCriteria[pageSize]=5` and check if `extension_attributes.split_cash_status` appears in the response
* Check that `extension_attributes.xml` is correctly declaring the `split_cash_status` attribute on `OrderInterface`


## Verification Checklist

* [ ] `split_*` columns visible in `sales_order` table
* [ ] REST endpoints return 401 (not 404) when called without auth
* [ ] Split payment UI renders at checkout when Cash is selected
* [ ] Validation messages work (overpayment, insufficient credit)
* [ ] Threshold guard blocks orders > $100
* [ ] Placed order has `pending_payment` status and App Builder comments
* [ ] `simulate-split-payment.mjs list` shows the test order with split amounts
* [ ] `simulate-split-payment.mjs accept <id>` moves order to `processing` with invoice and shipment
* [ ] `simulate-split-payment.mjs decline <id>` cancels the order
* [ ] Demo dashboard lists pending orders and accept/decline work from the UI


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

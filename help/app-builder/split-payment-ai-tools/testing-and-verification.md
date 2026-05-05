---
title: 分割払いPOC：テストと検証ガイド
description: 分割支払いPOCを確認する方法を説明します。 Commerceのインストール、REST、チェックアウト、しきい値、シミュレーションの承認と拒否、デモダッシュボード、App Builder ログ。
feature: App Builder, Configuration, Extensibility, Paas, Payments, REST, Orders
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 359
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 8dfbf2694378aae76c91afa11bfee7d93077d8ba
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 0%

---

# 分割払いPOC：テストと検証ガイド

このガイドでは、すべてのコンポーネントがテストする順序で正しく動作することを確認する方法について説明します。 下部（Commerce モジュール）から始めて作業する（App Builder）。


## 手順1 - Commerce Moduleのインストールを確認する

`bin/magento setup:upgrade`および`bin/magento setup:di:compile`の実行後：

```bash
# Confirm module is enabled
bin/magento module:status Client_SplitPayment

# Confirm db_schema columns were added
bin/magento doctrine:schema:validate
# or check directly:
mysql -u <user> -p <dbname> -e "DESCRIBE sales_order;" | grep split
```

予想される出力：`sales_order`に`split_`で始まる4列が表示されます。


## 手順2 - Commerce管理者設定の確認

Commerce Adminの場合：
1. **[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Sales]** > **[!UICONTROL Payment Methods]** — **[!UICONTROL Cash On Delivery]**&#x200B;がタイトル `Cash`で有効になっていることを確認します
2. **[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Customers]** > **[!UICONTROL Customer Configuration]** > **[!UICONTROL Store Credit Options]** – 有効化を確認
3. テスト用のお客様がストアクレジットを持っていることを確認します：**[!UICONTROL Customers]** > **[!UICONTROL All Customers]** > *[お客様]* > **[!UICONTROL Store Credit]**


## ステップ 3 - REST エンドポイントにアクセスできることを確認する

curlを使用して、エンドポイントが応答することを確認します（エンドポイントは不正なリクエストを拒否しますが、401は正しくルーティングされていることを確認します）。

```bash
# Should return 401 (not 404) — endpoint exists but requires auth
curl -X POST https://your-store.example.com/rest/V1/split-payment/orders/1/cash-received

# Should return 200 or session-based response — anonymous endpoint
curl -X POST https://your-store.example.com/rest/V1/split-payment/set \
  -H "Content-Type: application/json" \
  -d '{"storeCreditAmount": 0, "cashAmount": 0}'
```


## ステップ 4 - チェックアウト UIをテストする

1. テスト顧客（ストアクレジットを持つ顧客）としてストアフロントにログインします
2. 商品をカートに追加する（送料+税込み後の合計が100 ドル未満）
3. チェックアウトに進む
4. 支払い手順で、**Cash** （または代金引換）を選択します
5. 分割された支払いフィールドが表示されることを確認します。
   * ストアのクレジット残高が表示されます
   * 注文合計が事前に入力された現金金額フィールド
   * &quot;Store credit toward this order&quot; フィールドに$0.00が表示されている（現金=注文総額から）
6. 現金の金額を減らします（例：注文が50 ドルの場合は10 ドル入力）。
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

**修正：**
* `io_events.xml`がフィールドリストに`entity_id`を含むことを確認してください
* アクションで、`JSON.stringify(params)`を一時的にログに記録して、ペイロードの完全な形状を確認します
* `extractValue()`関数が適切な入れ子レベルを見つけていることを確認してください

### デモダッシュボードに注文が表示されない

**原因：** Commerce REST `orders`の検索条件が注文を返していないか、REST応答に`split_cash_status` フィールドが含まれていません。

**修正：**
* `OrderRepositoryPlugin`が拡張機能の属性を正しく読み込んでいることを確認してください
* 直接テスト：`GET /rest/V1/orders?searchCriteria[pageSize]=5`。応答に`extension_attributes.split_cash_status`が表示されているかどうかを確認します
* `extension_attributes.xml`が`OrderInterface`で`split_cash_status`属性を正しく宣言していることを確認してください


## 確認用チェックリスト

* [ ] `split_*`列が`sales_order` テーブルに表示されます
* [ ]個のREST エンドポイントは、認証なしで呼び出されると401 （404ではない）を返します
* [ ] 「現金」が選択されている場合、分割支払UIはチェックアウト時にレンダリングされます
* [ ]検証メッセージが機能します（過払い、クレジット不足）
* [ ]しきい値の監視ブロック注文> $100
* [ ]件の注文には`pending_payment`件のステータスとApp Builder コメントがあります
* [ ] `simulate-split-payment.mjs list`は、分割金額を含むテスト注文を表示します
* [ ] `simulate-split-payment.mjs accept <id>`様は、請求書と発送を含む注文を`processing`様に移動します
* [ ] `simulate-split-payment.mjs decline <id>`は注文をキャンセルします
* [ ]件のデモダッシュボードには、保留中の注文とUIからの作業の承認/拒否が一覧表示されます


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

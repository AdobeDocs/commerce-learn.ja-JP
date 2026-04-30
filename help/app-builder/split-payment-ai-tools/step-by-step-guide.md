---
title: 分割支払いPOC ステップバイステップの実装ガイド
description: モジュール、環境、オーケストレーター、検証、オプションのUIなど、セットアップ完了後に分割支払いPOCを順番に実装する方法について説明します。
feature: App Builder, Eventing, Extensibility
topic: Commerce, Architecture, App Builder, Development
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 480
jira: KT-20902
last-substantial-update: 2026-04-30T00:00:00Z
source-git-commit: 27811c05f066c95a60ac309472d6488295fb2c96
workflow-type: tm+mt
source-wordcount: '1150'
ht-degree: 0%

---

# 分割支払いPOC ステップバイステップの実装ガイド

このページは、実装のチェックリストです。 [overview](./overview.md)と[の完全なデモ &#x200B;](./full-demo.md)をすでに視聴しており、Commerce サイトとApp Builder プロジェクトの両方の準備ができていることを前提としています。 個々のチュートリアルページには、以下の各トピックの詳細が含まれています。このページを使用して、何をすべきか、どのような順序で行うべきかを確認してください。

## 始める前に

プロンプトまたはコマンドを実行する前に、次のすべての項目を確認します。 [前提条件と設定](./prerequisites-and-setup.md) ページでは、各項目について詳しく説明しています。

**Commerce側**

* [ ] Adobe Commerce 2.4.5以降
* [ ]個のI/O イベントを有効にしてデプロイしました（Commerce Cloudのみ – `.magento.env.yaml`で`ENABLE_EVENTING: true`が必要）
* [ ] **代金引換**&#x200B;が管理画面で有効になり、タイトルが`Cash`に正確に設定されています
* [ 管理画面で] ストアクレジットが有効になりました
* [ ] テストのお客様は、少なくとも$50 ストアクレジットを持っています
* [ ] Commerce統合が作成され、アクティブ化され、4つのOAuth値がすべて安全に保存されました

**App Builder側**

* [ ] App Builder プロジェクトがAdobe Developer Consoleに存在します
* [ ] Adobe I/O Events サービスがワークスペースに追加されました
* [ ] Commerceがイベントプロバイダーとして接続されました
* [ ] `aio login`が完了しました。`aio app use`で正しいワークスペースが選択されました
* [ ] Node.js 18以降がインストールされ、`aio` CLIがインストールされました（`npm install -g @adobe/aio-cli`）

上記の項目が見つからない場合は、ここで停止して、最初に完了してください。

## フェーズ 1 - Commerceモジュールの構築

**処理：** チェックアウト UI、ストアクレジットアプリケーション、REST エンドポイント、およびI/O イベントのサブスクリプションを処理する`Client_SplitPayment` PHP モジュールを生成します。 これはシンなCommerce内アダプタです。すべてのオペレーターワークフローはApp Builderに残ります。

### ステップ 1.1 - プロジェクトのプロンプトをカスタマイズする

[Commerce モジュール AI プロンプト &#x200B;](./commerce-module-prompt.md) ページを開き、プロンプトをコピーする前に次の点に注意してください。

* プロンプトで`Client`を実際のベンダー名に置き換えます
* 別のモジュール名が必要な場合は、`SplitPayment`を置換します
* テーマがLumaでない場合、`LayoutProcessorPlugin`のインジェクションパスとRequireJS マッピングを調整する必要がある場合があります
* 代金引換方法で`cashondelivery`以外のコードを使用している場合は、`payment-method-helper.js`を更新してください

### ステップ 1.2 - プロンプトを実行する

完全なプロンプトを&#x200B;**プロンプト開始**&#x200B;から&#x200B;**プロンプト終了**&#x200B;までコピーし、カーソル（Claudeを含む）またはClaudeに直接貼り付けます。 Commerce プロジェクトのルートから実行して、AIが`app/code/`の下にファイルを作成できるようにします。

### ステップ 1.3 - モジュールを有効にしてインストールする

Commerce プロジェクトルートから次のコマンドを実行します。

```bash
bin/magento module:enable Client_SplitPayment
bin/magento setup:upgrade
bin/magento setup:di:compile
bin/magento setup:static-content:deploy -f
bin/magento cache:flush
```

### 手順1.4 - モジュールが正しくインストールされていることを確認する

```bash
# Confirm the module is active
bin/magento module:status Client_SplitPayment

# Confirm the split_ columns were added to sales_order
mysql -u <user> -p <dbname> -e "DESCRIBE sales_order;" | grep split
```

5つの列（`split_store_credit_amount`、`split_cash_amount`、`split_cash_status`、`split_sc_invoice_id`、および`split_cash_invoice_id`）が表示されます。

次に、ストアクレジットを持つログイン顧客が支払い方法として&#x200B;**Cash**&#x200B;を選択すると、チェックアウト時に分割された支払いフィールドが表示されることをブラウザーで確認します。

## フェーズ 2 – 環境変数の設定

**機能：** App Builder オーケストレーター、シミュレーション スクリプト、および（オプションで）Experience Cloud UI拡張機能で使用される資格情報ファイルを準備します。 Commerce統合の同じ4つのOAuth値が、すべての`.env` ファイルで使用されます。

コンポーネントごとの変数の完全なリストについては、[環境変数リファレンス &#x200B;](./env-reference.md)を参照してください。

### 手順2.1 - オーケストレーター`.env`を作成します。

`split-payment-orchestrator/` ディレクトリ内：

```bash
cp .env.example .env
```

すべての値を入力：

| 変数 | どこから入手するか |
|---|---|
| `COMMERCE_BASE_URL` | ストア URL – 末尾のスラッシュなし |
| `COMMERCE_CONSUMER_KEY` | Commerce管理者/システム/統合機能 |
| `COMMERCE_CONSUMER_SECRET` | 同一 – アクティベーション時のみ表示 |
| `COMMERCE_ACCESS_TOKEN` | 同一 |
| `COMMERCE_ACCESS_TOKEN_SECRET` | 同一 |
| `PAYMENT_THRESHOLD` | Commerceの`split_payment/general/threshold`と一致する必要があります（デフォルト：`100`） |
| `DEMO_UI_SECRET` | オプション – 設定されている場合、ダッシュボード URLで`?secret=`として必要 |

### 手順2.2 - シミュレーションスクリプト `.env`を作成します。

```bash
cp commerce-backend-ui-1/.env.simulation.example commerce-backend-ui-1/.env.simulation
```

`COMMERCE_BASE_URL`と4つのOAuth値（上記と同じ資格情報）を入力します。

## フェーズ 3 - App Builder Orchestratorの構築

**これが何か：**&#x200B;は、`split-payment-orchestrator` App Builder プロジェクトを生成します：`payment-orchestrator` I/O イベントコンシューマー、`payment-accept`および`payment-decline` web アクション、および`demo-dashboard` オペレーターUI。

### ステップ 3.1 - オーケストレータープロンプトを実行する

[App Builder Orchestrator AI プロンプト &#x200B;](./orchestrator-prompt.md)を開きます。 完全なプロンプトをコピーし、`split-payment-orchestrator/` ディレクトリから実行します。

### 手順3.2 – 依存関係のインストールとデプロイ

```bash
cd split-payment-orchestrator
npm install
# Confirm .env is filled in (from Phase 2, Step 2.1)
aio app deploy
```

デプロイメント後、`aio app deploy`はアクション URLを出力します。 `demo-dashboard` URLに注意してください。フェーズ 4で必要です。

### ステップ 3.3 - イベント登録の確認

Adobe Developer Consoleで、App Builder プロジェクトワークスペースを開きます。 **イベント**&#x200B;で、`Split payment — sales order place before`登録が存在し、`payment-orchestrator` アクションにバインドされていることを確認します。

## フェーズ 4：フロー全体のテスト

確認の手順を順番に実行します。 [&#x200B; テストと検証ガイド &#x200B;](./testing-and-verification.md)には、以下の各ステップの完全なcurl コマンド、シミュレーションスクリプトの使用方法、およびトラブルシューティングのリファレンスが記載されています。

### 手順4.1 - REST エンドポイントがルーティング可能であることを確認する

```bash
# Expect 401, not 404 — the endpoint exists but requires auth
curl -X POST https://your-store.example.com/rest/V1/split-payment/orders/1/cash-received
```

### ステップ 4.2 - チェックアウト UIのテスト

1. テスト顧客（ストアクレジットを持つ顧客）としてストアフロントにログインします
2. 送料と税金後の合計が100 ドル未満の商品をカートに追加する
3. 支払い手順で、**Cash**&#x200B;を選択します
4. 分割支払いフィールドが表示されていることを確認します：残高が表示され、現金が事前入力され、ストアクレジットが$0で表示されます
5. 現金金額を減らし、店舗のクレジット部分が正しく更新されていることを確認します

### ステップ 4.3 – しきい値ガードのテスト

合計が100 ドルを超える商品を追加し、チェックアウトに進み、「現金」を選択して、注文を試みます。 エラー`"Payment could not be processed. Please try again or contact support."`が表示される必要があります。

### ステップ 4.4 - テスト分割支払い注文を配置する

100 ドル未満でカートを作成し、現金/店舗のクレジット分割を入力して、注文します。 Commerce Adminで、次の項目を確認します。

* 注文ステータスは`pending_payment`です
* App Builderからの2つのコメントが順番に表示されます。
   1. `"Cash payment of $X.XX pending. Awaiting admin confirmation."`
   2. `"Split payment orchestration completed. Order awaiting cash confirmation."`

App Builder コメントが表示されない場合は、続行する前に`aio app logs`でログを確認してください。

### ステップ 4.5 - シミュレーションスクリプトを使用したテスト承認

```bash
# Find your order's entity_id
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs list

# Accept the payment
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs accept <entity_id>
```

Commerce Adminで、次の項目を確認します。

* 注文状況が`processing`に変更されました
* 履歴コメント：`"Cash payment of $X.XX received."`
* 「請求書」タブに表示される現金請求書
* 「出荷」タブに表示される出荷（該当する場合）

### ステップ 4.6 - シミュレーションスクリプトによるテストの低下

別のテスト注文を行い、次の手順を実行します。

```bash
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs decline <entity_id>
```

注文状況が`canceled`、`split_cash_status`が`declined`であることを確認してください。

### ステップ 4.7 - デモダッシュボードのテスト

手順3.2のダッシュボード URLを開きます。 システムで保留中の注文の場合：

1. 注文が保留中リストに表示されることを確認します
2. **Accept**&#x200B;をクリックし、Commerceの`processing`に移動する注文を確定します
3. 新しい注文を行い、ダッシュボードに戻り、**辞退**&#x200B;をクリックして、キャンセルを確認します

## フェーズ 5 （オプション） — Experience Cloud UI拡張機能を追加します

このフェーズでは、スタンドアロン `demo-dashboard`を使用する代わりに、Commerce管理シェルにオペレーターダッシュボードを埋め込みます。 スタンドアロンダッシュボードがニーズを満たす場合は、このフェーズをスキップしてください。

### ステップ 5.1 – 前提条件の確認

Experience Cloud UI拡張機能には、OAuth統合値に加えてIMS資格情報が必要です。 プロンプトを実行する前に、[Experience Cloud UI拡張機能AI プロンプト &#x200B;](./experience-cloud-ui-prompt.md) ページを読み、`OAUTH_CLIENT_ID`および関連するIMS変数に注意してください。

### ステップ 5.2 - UI拡張機能プロンプトを実行する

完全なプロンプトを&#x200B;**プロンプト開始**&#x200B;から&#x200B;**プロンプト終了**&#x200B;までコピーし、`commerce-checkout-starter-kit/` ディレクトリから実行します。

### ステップ 5.3 - デプロイ

```bash
cd commerce-checkout-starter-kit
npm install
cp .env.dist .env
# Fill in IMS credentials and COMMERCE_INTEGRATION_* credentials
aio app deploy
```

## クイックトラブルシューティングリファレンス

| 症状 | 最初にチェックすべきこと |
|---|---|
| 決済時に分割支払いフィールドが表示されない | ブラウザーコンソールでRequireJS エラーが発生しました。`bin/magento cache:flush`も実行します |
| `"Payment could not be processed"`件のプレース注文 | `PlaceOrderPlugin`または`BalanceManagementInterface` – 一時デバッグのログを`PlaceOrderPlugin`に追加 |
| 注文に関するApp Builder コメントはありません | `aio app logs`を実行して、イベントが発生したかどうか、およびアクションが返されたものを確認します |
| App BuilderのCommerce RESTからの`403` | Fastly IP許可リストに加える：最初にシミュレーションスクリプトを使用してテストします。これが機能する場合、問題はエグレス IP ブロックです |
| Commerceの`"The signature is invalid"` | `COMMERCE_BASE_URL`の末尾にスラッシュがないことを確認してください。統合がアクティブになっていることを確認してください |
| デモダッシュボードに注文が表示されない | `extension_attributes.split_cash_status`が直接`GET /rest/V1/orders`応答に表示されていることを確認してください |

トラブルシューティングの詳細については、[&#x200B; テストと検証ガイド &#x200B;](./testing-and-verification.md#common-issues-and-fixes)を参照してください。

{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

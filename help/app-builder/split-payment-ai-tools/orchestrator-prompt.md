---
title: 分割支払いPOC — App Builder Orchestrator AI プロンプト
description: このプロンプトを使用してsplit-payment-orchestrator アプリを構築する方法を説明します。 I/O イベント、payment-orchestrator、web アクション、デモダッシュボード、aio アプリのデプロイ。
feature: App Builder, Configuration, Eventing, Extensibility, Paas, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 421
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '927'
ht-degree: 0%

---

# 分割支払いPOC: App Builder orchestrator AI プロンプト

このページを使用して、**split-payment-orchestrator** プロジェクトを生成するプロンプト全体をコピーします。**payment-orchestrator** I/O イベントコンシューマー、**payment-accept**&#x200B;および&#x200B;**payment-decline** web アクション、**demo-dashboard**、およびCommerce REST クライアント。

## このプロンプトの使用方法

**プロンプト開始**&#x200B;から&#x200B;**プロンプト終了**&#x200B;までのすべての項目をカーソル（Claudeを含む）または直接Claudeにコピーします。 `split-payment-orchestrator/` ディレクトリ （App Builder プロジェクトのルート）からプロンプトを実行します。

## 実行する前

* [分割支払いPOC：前提条件と環境の設定](./prerequisites-and-setup.md)を完了します。
* [分割支払いPOC：環境変数リファレンス ](./env-reference.md)と`.env` ファイルをプロジェクトで準備します。


## プロンプト

**プロンプト開始**


Adobe Commerceで分割支払いのオーケストレーションを行うためのAdobe App Builder アプリケーション全体を生成します。 このアプリケーションは、CommerceからI/O イベントを受け取り、分割された支払い決定を処理し、REST経由でCommerceに呼び出します。

**プロジェクト：** `split-payment-orchestrator`
**ランタイム：** Node.js 18
**主要な依存関係：** `@adobe/aio-sdk ^6.0.0`、`got ^11.8.6`、`oauth-1.0a ^2.2.6`

以下のすべてのファイルを生成します。 アプリケーションはAdobe I/O Runtime （`aio app deploy`）で動作する必要があります。


### 生成するファイル構造

```
split-payment-orchestrator/
├── package.json
├── app.config.yaml
├── .env.example
└── actions/
    ├── payment-orchestrator/
    │   ├── index.js         ← I/O Event entry point
    │   ├── commerce-client.js
    │   ├── threshold.js
    │   ├── cash-payment.js
    │   ├── order-update.js
    │   └── store-credit.js  ← Deprecated stub; kept for reference
    ├── payment-accept/
    │   └── index.js
    ├── payment-decline/
    │   └── index.js
    └── demo-dashboard/
        └── index.js
```


### `package.json`

```json
{
  "name": "split-payment-orchestrator",
  "version": "1.0.0",
  "private": true,
  "description": "Adobe App Builder action — split payment PoC orchestrator",
  "engines": { "node": ">=18" },
  "scripts": {
    "deploy": "NODE_OPTIONS=--disable-warning=DEP0040 aio app deploy",
    "aio": "NODE_OPTIONS=--disable-warning=DEP0040 aio"
  },
  "dependencies": {
    "@adobe/aio-sdk": "^6.0.0",
    "@adobe/exc-app": "^1.5.9",
    "got": "^11.8.6",
    "oauth-1.0a": "^2.2.6"
  }
}
```


### `app.config.yaml`

`split_payment_orchestrator` パッケージで4つのアクションを定義します。

**`payment-orchestrator`**
* `web: "no"` （I/O イベントトリガーのみ – HTTP経由で直接呼び出すことはできません）
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* 環境からの入力：`LOG_LEVEL`、`COMMERCE_BASE_URL`、`COMMERCE_CONSUMER_KEY`、`COMMERCE_CONSUMER_SECRET`、`COMMERCE_ACCESS_TOKEN`、`COMMERCE_ACCESS_TOKEN_SECRET`、`PAYMENT_THRESHOLD`

**`payment-accept`**
* `web: "yes"` （HTTP web アクション – ダッシュボードまたはERPで呼び出し可能）
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* 同じCommerce資格情報の入力（なし`PAYMENT_THRESHOLD`）

**`payment-decline`**
* `web: "yes"`
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* 同じCommerce資格情報の入力

**`demo-dashboard`**
* `web: "yes"`
* `runtime: nodejs:18`
* `require-adobe-auth: false` ← ダッシュボードは一般に公開されています（設定されている場合は`DEMO_UI_SECRET`によってのみ保護されます）
* 入力：すべてのCommerce資格情報+ `DEMO_UI_SECRET`、`DEMO_UI_BASE_URL`

**イベント登録** （`events.registrations`以下）:

```yaml
Split payment — sales order place before:
  description: Invokes payment-orchestrator when an order is about to be placed
  events_of_interest:
    - provider_metadata: dx_commerce_events
      event_codes:
        - com.adobe.commerce.observer.sales_order_place_before
  runtime_action: split_payment_orchestrator/payment-orchestrator
```


### `actions/payment-orchestrator/commerce-client.js`

Adobe Commerce用のShared OAuth 1.0a REST クライアント。 実装：

**`createCommerceClient(params, logger)`** — `{ request, baseUrl }`を返します

* `params`から`COMMERCE_BASE_URL`、`COMMERCE_CONSUMER_KEY`、`COMMERCE_CONSUMER_SECRET`、`COMMERCE_ACCESS_TOKEN`、`COMMERCE_ACCESS_TOKEN_SECRET`を読み取ります
* 資格情報が見つからないとスローします
* `oauth-1.0a`を`HMAC-SHA256`と共に使用します（Node.js `crypto.createHmac`）
* `got@11`を使用しています（`got@12+`ではなく） – プロジェクトは`prefixUrl = ${baseUrl}/rest/V1/`でCJSを使用しています）
* `beforeRequest` フックを介して`Authorization` ヘッダーを追加
* `request(method, path, options)` — パスを正規化し（`/`の先頭にストリップ）、`{ statusCode, body }`を返します
* `throwHttpErrors: false` — 4xx/5xxにスローしません。常にステータスコードを返します


### `actions/payment-orchestrator/threshold.js`

**`evaluateThreshold({ orderTotal, storeCreditAmount, cashAmount, params, logger })`** — `{ pass: boolean, reason: string }`を返します

ロジック：
1. `params.PAYMENT_THRESHOLD`を読み取り、浮動小数として解析します。見つからない場合はデフォルトで`100`に、NaNまたは≤が0になります
2. `orderTotal > threshold`の場合：`{ pass: false, reason: 'PAYMENT_THRESHOLD_EXCEEDED' }`を返します
3. `Math.abs((storeCreditAmount + cashAmount) - orderTotal) > 0.02`の場合：`{ pass: false, reason: 'SPLIT_AMOUNT_MISMATCH' }`を返します
4. それ以外：`{ pass: true, reason: '' }`を返します


### `actions/payment-orchestrator/cash-payment.js`

**`recordCashPending({ commerce, orderId, cashAmount, logger })`** — `{ ok: boolean, error? }`を返します

* `POST orders/${orderId}/comments`への呼び出し条件：

  ```json
  {
    "statusHistory": {
      "comment": "Cash payment of $X.XX pending. Awaiting admin confirmation.",
      "entity_name": "order",
      "parent_id": "<orderId>",
      "is_visible_on_front": true,
      "is_customer_notified": false,
      "status": "pending_payment"
    }
  }
  ```

* 2xxで`{ ok: true }`を返します。それ以外の場合は`{ ok: false, error: { code, message } }`を返します
* try/catchで回り込む。エラーオブジェクトを返し、スローしない


### `actions/payment-orchestrator/order-update.js`

**`updateOrderAfterOrchestration({ commerce, orderId, success, detail, logger })`** — `{ ok: boolean, error? }`を返します

* `success`さんが履歴コメント `"Split payment orchestration completed. Order awaiting cash confirmation."`を投稿した場合
* `!success`：投稿`"Payment could not be processed. Please try again or contact support."` – 顧客に表示されるコメントに`detail`を含めません。`detail`を内部専用で記録します
* `{ ok: boolean, error? }`を返します – スローしません


### `actions/payment-orchestrator/store-credit.js`

**`applyStoreCredit({ commerce, cartId, amount, logger })`** – 非推奨のno-op実装

このファイルをJSDoc `@deprecated`の通知に含めて、次のことを説明します。
> オーケストレーターは、RESTを介してストアクレジットを適用しなくなりました。 ストアのクレジットは、チェックアウト時に`BalanceManagementInterface::apply()`を使用してCommerce PHP モジュール（`PlaceOrderPlugin`）に適用されます。このモジュールでは、アクティブなカートが必要です。 App BuilderがI/O イベントを受け取るまでに、カートは非アクティブになります。 このファイルは、参照用またはストアクレジットが注文の後に適用されるカスタムフロー用に保持されます。

開発者がパターンを学習できるように、作業中の実装（他のモジュールと同じ形状）を維持しますが、現在のフローで使用されていないとして明確にマークします。


### `actions/payment-orchestrator/index.js`

I/O イベントエントリポイント。 `async function main(params)`を実装します。

**ペイロード抽出：**

Adobe Commerce I/O Eventsは、ペイロードを複数の形状で配信する場合があります。 次のパスを順番にチェックして、注文値オブジェクトを抽出します。
1. `params.__ow_body` （必要に応じてJSON文字列を解析） → `.event.data.value`
2. `params.data.value`
3. `params.value`
4. `params.body.event.data.value`
5. `params`自体にフォールバック

**値からのフィールド抽出：**
* `orderId = value.entity_id || value.id`
* `orderTotal = parseFloat(value.grand_total ?? value.base_grand_total ?? value.subtotal ?? '0')`
* `value.extension_attributes`から金額を分割（`extension_attributes`と`extensionAttributes`の両方を確認）:
   * `storeCredit = parseFloat(ext.split_store_credit_amount ?? value.split_store_credit_amount ?? '0')`
   * `cash = parseFloat(ext.split_cash_amount ?? value.split_cash_amount ?? '0')`

**フロー：**
1. 値を抽出します。`orderId`が見つからない場合は、エラーをログに記録して`{ statusCode: 200, body: { ok: false, message: PUBLIC_ERROR } }`を返します
2. `evaluateThreshold(...)`を呼び出す – 失敗した場合は、同じ200件の応答を記録して返します
3. `createCommerceClient(params, logger)`を呼び出す – 失敗した場合は200 エラーを返します
4. `storeCredit > 0`の場合、チェックアウト時にストアクレジットが適用されたことをログに記録します（REST呼び出しは不要）
5. `recordCashPending(...)`を呼び出す – 失敗した場合は、`updateOrderAfterOrchestration(..., success: false)`に電話して200 エラーを返します
6. `updateOrderAfterOrchestration(..., success: true)`に電話
7. `{ statusCode: 200, body: { ok: true, message: 'processed' } }`を返す

**重要：**&#x200B;常に`statusCode: 200`を返す – I/O Runtimeは200件以外の応答を再試行します。これにより、注文処理が重複します。 誤りが体に報告されます。

**`PUBLIC_ERROR`定数：** `"Payment could not be processed. Please try again or contact support."` – すべての外部向けエラーメッセージに使用されます。


### `actions/payment-accept/index.js`

HTTP web アクション。 `POST /V1/split-payment/orders/:orderId/cash-received`を呼び出します。

**注文ID解決：** `params.orderId`、次に`params.payload.orderId`、次に`params.__ow_body` （解析されたJSON）を確認します。 見つからない場合は400を返します。

**フロー：**
1. `orderId`を解決。見つからない場合は400を返します
2. Init commerce クライアント。失敗した場合は500を返します
3. 空のJSON本文で`POST split-payment/orders/${orderId}/cash-received`を呼び出す
4. 2xxの場合：`{ statusCode: 200, body: { ok: true, orderId, message: 'accepted' } }`を返します
5. エラーの場合：ログを記録して`{ statusCode: 200, body: { ok: false, message: PUBLIC_ERROR } }`を返します


### `actions/payment-decline/index.js`

HTTP web アクション。 `payment-accept`と同じパターンですが、`POST /V1/split-payment/orders/:orderId/cash-decline`を呼び出します。

成功時に`{ ok: true, orderId, message: 'declined' }`を返します。


### `actions/demo-dashboard/index.js`

自己完結型のデモオペレーターダッシュボード。 保留中の現金注文を一覧表示し、承認/拒否アクションをトリガーするHTML ダッシュボードを提供します。 これは、HTML UIとJSON APIの両方を提供する単一のweb アクションです。

**セキュリティ：**
* オプションの`DEMO_UI_SECRET` チェック：設定する場合、すべてのリクエストに`?secret=<value>` クエリパラメーターまたは`x-demo-secret` ヘッダーが必要です。 見つからない場合や間違っている場合は401を返します。
* `DEMO_UI_SECRET`が設定されていない場合は警告を記録します（ダッシュボードが保護されていません）

**ルーティング （HTTP メソッド + パス/本文に基づく）:**

アクションはすべてのリクエストを受信します。 インテントの決定方法：
* `params.__ow_method` （GET/POST）と`params.__ow_path`またはアクションパラメーター
* HTML ダッシュボードに対する操作→ない`GET`
* `GET` （`action=list` パラメーター）は、保留中の注文のJSON リスト→返します
* `POST`と`action=accept`および`orderId`は`payment-accept` ロジック→呼び出します（またはCommerce REST呼び出しをインライン化します）
* `POST`と`action=decline`および`orderId`→呼び出し`payment-decline` ロジック

**保留中の注文を取得しています：**
* Commerce RESTから`GET orders?searchCriteria[pageSize]=50`を呼び出す
* `extension_attributes.split_cash_status === 'pending'` AND注文がターミナル状態（`complete`、`closed`、`canceled`、`cancelled`）にない注文にフィルターを適用します
* メモリ内の最新の順に並べ替え
* 最大20個まで返します（`limit` パラメーターで設定可能）

**HTML ダッシュボード：**
ダッシュボードは、`Content-Type: text/html`で返されるHTML文字列です。 次のようなことが必要です。
* 表の保留中の現金注文のリスト：注文# （increment_id）、entity_id、顧客名、現金金額、店舗クレジット金額、日付
* アクションの独自のAPI エンドポイントを呼び出す注文ごとに&#x200B;**Accept**&#x200B;および&#x200B;**辞退** ボタンを指定します
* 成功/エラーの応答をインラインで表示
* 更新ボタンを含める
* 使用可能なスタイルを十分に設定します（最小限のCSSで問題ありません。ランタイムでのCORSの問題を回避するための外部CDN依存関係はありません）。
* `DEMO_UI_SECRET`が設定されていない場合は、警告バナーを表示する

**UIでのエラーの表示：**
Commerce REST呼び出しが失敗した場合は、HTTP ステータスとCommerce エラー本文の簡単な説明を含めます（sanitize — HTML タグをストリップし、500文字まで切り捨てます）。 資格情報は公開しないでください。

**応答ヘルパー：**

```javascript
function jsonResponse(statusCode, obj, extraHeaders = {}) { ... }
function htmlResponse(html) { ... }
```


### `.env.example`

```dotenv
# Commerce REST base URL — no trailing slash
COMMERCE_BASE_URL=

# OAuth 1.0a integration credentials (Admin → System → Integrations)
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=

# PoC threshold — must match split_payment/general/threshold in Commerce (default: 100)
PAYMENT_THRESHOLD=100

LOG_LEVEL=info

# Demo dashboard optional shared secret
DEMO_UI_SECRET=
DEMO_UI_BASE_URL=
```


### Deploy コマンド

すべてのファイルを生成した後、`split-payment-orchestrator/` ディレクトリから：

```bash
npm install
cp .env.example .env
# Edit .env with your credentials
aio app deploy
```

デプロイメント後、`aio app deploy`によって印刷されたアクション URLに注意してください。 `demo-dashboard` URLは、オペレーターダッシュボードにアクセスする場所です。


### プロンプトの終了


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

---
title: 分割払いPOC — Experience Cloud UI拡張機能AI プロンプト
description: このオプションのプロンプトを使用して、Commerce管理者に分割支払いを埋め込む方法（管理UI SDK、IMS、OAuth、承認と拒否、およびシミュレーションスクリプト）について説明します。
feature: App Builder, Admin Workspace, Extensibility, Paas, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 192
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 0%

---

# 分割支払いPOC: Experience Cloud UI拡張機能AI プロンプト

これは、`commerce-checkout-starter-kit`と`commerce-backend-ui-1` パターンを使用して、**[!UICONTROL Adobe Commerce]**&#x200B;管理シェル （Experience Cloud）に分割支払い注文パネルを埋め込むオプションの手順です。 App Builder orchestratorのスタンドアロン [ デモダッシュボード ](./orchestrator-prompt.md)は、管理者シェルの統合なしで、同じ承認と拒否のフローをカバーしています。

## このプロンプトの使用方法

**プロンプト開始**&#x200B;から&#x200B;**プロンプト終了**&#x200B;までのすべての項目をカーソルまたはクロードにコピーします。 `commerce-checkout-starter-kit/` ディレクトリからプロンプトを実行します。

## 実行する前

* このパスには、OAuth値に加えて&#x200B;**IMS**&#x200B;資格情報が必要です（`commerce-checkout-starter-kit`変数の[分割支払いPOC：環境変数リファレンス ](./env-reference.md)を参照）。
* 同じ`payment-accept`と`payment-decline`のビヘイビアーを比較する場合は、[分割支払いPOC: App Builder オーケストレーターAI プロンプト ](./orchestrator-prompt.md)を最初に実行します。UI拡張機能は、そのロジックを`COMMERCE_INTEGRATION_*`個の環境名で再利用します。


## プロンプト

**プロンプト開始**


分割支払いPOC用の`commerce-backend-ui-1`管理UI SDK拡張機能を生成しています。 この拡張機能は、Commerce Checkout Starter Kitの`@adobe/aio-app-dev-toolkit`と`@adobe/commerce-backend-ui-1`のパターンを使用して、分割された支払い注文ダッシュボードをAdobe Commerce管理シェルに埋め込みます。

**基本プロジェクト：** `commerce-checkout-starter-kit/`
**拡張ディレクトリ：** `commerce-checkout-starter-kit/commerce-backend-ui-1/`


### この拡張機能は

1. **管理者注文グリッドパネル** — Commerce管理シェルのカスタムメニュー項目で、`split_cash_status = 'pending'`の分割支払い注文を一覧表示します
2. **注文詳細ビュー** – 分割支払いの内訳（現金金額、店舗クレジット金額、ステータス）を注文と一緒に表示します
3. **アクションの受け入れ/拒否** — OAuth 1.0a経由で`payment-accept`および`payment-decline`個のApp Builder アクションを呼び出すボタン


### 生成するファイル構造

```
commerce-checkout-starter-kit/commerce-backend-ui-1/
├── ext.config.yaml
├── README.md
├── .env.simulation.example
├── actions/
│   ├── utils.js
│   ├── commerce/
│   │   └── index.js           ← IMS-based Commerce REST (order listing)
│   ├── payment-accept/
│   │   ├── commerce-client.js ← OAuth 1.0a (accept/decline)
│   │   └── index.js
│   ├── payment-decline/
│   │   └── index.js
│   └── registration/
│       └── index.js
├── scripts/
│   ├── README.md
│   └── simulate-split-payment.mjs
└── web-src/
    ├── index.html
    ├── package.json
    ├── .parcelrc
    └── src/
        ├── index.jsx
        ├── index.css
        ├── utils.js
        ├── exc-runtime.js
        ├── config.json
        ├── constants/
        │   └── extension.js
        ├── components/
        │   ├── App.jsx
        │   ├── ExtensionRegistration.jsx
        │   ├── MainPage.jsx
        │   ├── SplitPaymentDashboard.jsx
        │   └── SplitPaymentOrderDetail.jsx
        └── hooks/
            └── useSplitPaymentOrders.js
```


### バックエンド操作

**`actions/commerce/index.js`** — IMS認証のCommerce REST
* 管理者UI SDK コンテキストによって提供されるIMS トークンを使用して、Commerce RESTを呼び出します
* `split_cash_status` フィルターを含む注文リストを取得します
* 注文リストをJSONとして返します

**`actions/payment-accept/commerce-client.js`** — OAuth 1.0a クライアント
* `split-payment-orchestrator/actions/payment-orchestrator/commerce-client.js`と同じ実装
* 接頭辞が付いた`COMMERCE_INTEGRATION_*`個の環境変数を使用します（IMS資格情報と区別するため）

**`actions/payment-accept/index.js`** — アクションを承認
* `split-payment-orchestrator/actions/payment-accept/index.js`と同じロジック
* OAuth 1.0a経由で`POST /V1/split-payment/orders/:orderId/cash-received`を呼び出す

**`actions/payment-decline/index.js`** — アクションを辞退
* `POST /V1/split-payment/orders/:orderId/cash-decline`を呼び出します

**`actions/registration/index.js`** – 管理者UI SDKの登録
* 拡張機能をCommerce管理シェルに登録します
* 分割支払いダッシュボードの「注文」の下にメニュー項目を追加します


### React フロントエンドコンポーネント

**`SplitPaymentDashboard.jsx`**
* Spectrum スタイルのテーブルに、保留中の分割支払い注文を一覧表示します
* 列：注文番号（increment_id）、日付、顧客、支払期限、店舗クレジット、ステータス
* 1行あたりのボタンの受け入れと拒否
* `web-src/src/utils.js`件のフェッチ ヘルパーを介してバックエンド アクションを呼び出す
* 読み込み/エラー状態を表示します。アクションの完了時に更新されます

**`SplitPaymentOrderDetail.jsx`**
* 1つの注文の分割支払の詳細を表示します
* 表示：キャッシュ金額、ストアクレジット金額、現在のsplit_cash_status

**`useSplitPaymentOrders.js`** — React フック
* `actions/commerce/index.js`からの分割支払い注文を取得します
* `{ orders, loading, error, refresh }`を返します


### シミュレーションスクリプト

**`scripts/simulate-split-payment.mjs`**

Commerce REST呼び出しを直接テストするためのNode.js ESM スクリプト（App Builderを経由しない）。 App Builder アクションと同じOAuth 1.0a署名を使用します。

コマンド：
* `node simulate-split-payment.mjs help` – 使用状況を表示
* `node simulate-split-payment.mjs list` – 分割支払いデータを使用した最近の注文のリスト
* `node simulate-split-payment.mjs show <orderId>` – 特定の注文の分割支払いフィールドを表示します（entity_id）
* `node simulate-split-payment.mjs accept <orderId>` — `cash-received` エンドポイントの呼び出し
* `node simulate-split-payment.mjs decline <orderId>` — `cash-decline` エンドポイントの呼び出し

`commerce-backend-ui-1/.env.simulation`から資格情報を読み取ります（`.env.simulation.example`からコピー）。

**`.env.simulation.example`:**

```dotenv
COMMERCE_BASE_URL=https://your-store.example.com
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=
```


### `ext.config.yaml`

拡張機能を次の場所で設定します。
* `commerce-backend-ui-1`拡張機能タイプ
* 4つのバックエンドアクション （`commerce`、`payment-accept`、`payment-decline`、`registration`）
* `registration`を除くすべてのアクションの`require-adobe-auth: true`
* 環境から`COMMERCE_INTEGRATION_*`資格情報のバインディングを入力


### ファイルの生成後

```bash
cd commerce-checkout-starter-kit
npm install
cp .env.dist .env
# Fill in IMS credentials and COMMERCE_INTEGRATION_* credentials
aio app deploy
```

### プロンプトの終了


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

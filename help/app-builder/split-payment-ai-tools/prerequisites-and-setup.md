---
title: 分割払いPOC – 前提条件と環境設定
description: Commerce、CODおよびストアクレジットの管理者、OAuth統合、I/O Events、App Builder、aio CLIを分割支払いビルドプロンプトの前に設定する方法について説明します。
feature: App Builder, Configuration, Eventing, Extensibility, Paas, Payments, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 262
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '741'
ht-degree: 1%

---

# 分割払いPOC：前提条件と環境の設定

ビルドプロンプトを実行する前に、このチュートリアルのすべての手順を完了してください。 1つのステップが見つからない場合は、チュートリアルの途中でフローが壊れる最も一般的な理由です。

## &#x200B;1. Adobe Commerceの要件

* Adobe Commerce **2.4.5以降** （オンプレミスまたはCommerce Cloud）
* Commerce プロジェクトへのGit アクセス（`app/code/`の下にモジュールを追加した場合）
* **[!UICONTROL Commerce Admin]**&#x200B;へのアクセス

### Commerce Cloudのみ：I/O イベントを有効にする

モジュールを追加する前に、以下を`.magento.env.yaml`に追加してデプロイします。

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

> **警告：**&#x200B;このデプロイは、I/O イベントモジュールの依存関係を解決する前に正常に終了する必要があります。


## &#x200B;2. Commerce管理者設定

他の何よりも前にこれらのステップを実施しましょう。 チェックアウト JavaScriptは、正確な文字列の一致に応じて異なります。

### 2a. 正確なタイトルで代金引換を有効にする

**[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Sales]** > **[!UICONTROL Payment Methods]** > **[!UICONTROL Cash On Delivery Payment]**

* **[!UICONTROL Enabled]**: **はい**
* **[!UICONTROL Title]**: **`Cash`** （この正確な文字列は、チェックアウト JavaScriptと一致します）

> **注：** ストアで別の代金引換（COD）実装またはタイトルを使用している場合は、Commerce モジュールで`payment-method-helper.js`を調整します。

### 2b. ストアクレジットを有効にする

**[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Customers]** > **[!UICONTROL Customer Configuration]** > **[!UICONTROL Store Credit Options]**

* **[!UICONTROL Enable Store Credit Functionality]**: **はい**

### 2c. テスト顧客にストアクレジットを追加する

**[!UICONTROL Customers]** > **[!UICONTROL All Customers]** > *[テスト用のお客様]* > **[!UICONTROL Store Credit]** > **[!UICONTROL Update Balance]**

店舗クレジットに少なくとも&#x200B;**$50**&#x200B;を追加してください。 合計100 ドル未満の注文でテストします。

### 2d. Commerceとの連携の構築

**[!UICONTROL System]** > **[!UICONTROL Integrations]** > **[!UICONTROL Add New Integration]**

* **[!UICONTROL Name]**: `Split Payment App Builder` （または任意の名前）
* 「**[!UICONTROL API]**」タブで、少なくとも次の権限を付与します。
   * `Magento_Sales::actions` （`cash-received` エンドポイントに必要）
   * `Magento_Sales::cancel` （`cash-decline` エンドポイントに必要）
   * 見積もりまたはカートの管理（全部または関連するサブセット）
   * **[!UICONTROL Customer balance]** （完全または関連するサブセット）

「**[!UICONTROL Save]**」、「**[!UICONTROL Activate]**」の順に選択します。

**4つの値をすべてコピーします。これらは1回のみ表示されます：**

| 値 | 環境変数 |
| --- | --- |
| Consumer Key | `COMMERCE_CONSUMER_KEY` |
| Consumer Secret | `COMMERCE_CONSUMER_SECRET` |
| アクセストークン | `COMMERCE_ACCESS_TOKEN` |
| アクセストークン秘密鍵 | `COMMERCE_ACCESS_TOKEN_SECRET` |

これらの値を安全に保存します。 App Builder `.env` ファイルごとに必要です。


## &#x200B;3. Adobe Developer ConsoleとApp Builder

* Adobe Developer Console組織へのアクセス
* **App Builder プロジェクト** （新規または再利用したもの）
* ワークスペースが設定されました。プロンプトは&#x200B;**[!UICONTROL Stage]**&#x200B;と仮定します
* **[!UICONTROL Adobe I/O Events]**&#x200B;がワークスペースにサービスとして追加されました
* **Commerce**&#x200B;がワークスペースのイベントプロバイダーとして接続されました

概念実証で使用されるイベント コード：`com.adobe.commerce.observer.sales_order_place_before`

Commerceをイベントプロバイダーとして接続していない場合は、Commerce拡張ガイドの「[CommerceでAdobe I/Oにイベントを出力するように設定](https://developer.adobe.com/commerce/extensibility/events/configure-commerce/){target="_blank"}」を参照してください。


## &#x200B;4. ローカル開発環境

```bash
# Required versions
node --version    # 18.x or later
npm --version     # any recent version

# Adobe I/O CLI
npm install -g @adobe/aio-cli

# Authenticate
aio login

# Select your project and workspace
aio app use
# Confirm the org, project, and workspace shown are correct
```


## &#x200B;5. 知っておくべき2つのプロジェクトディレクトリ

このチュートリアルでは、2つの別々のディレクトリルートを使用します。 分離する：

**Commerce プロジェクトのルート** （Magento Git リポジトリ）:

```text
<commerce-root>/
└── app/code/Client/SplitPayment/   ← Module goes here
```

**App Builder プロジェクトのルート** （ソースパッケージの`split-payment-orchestrator` フォルダー、または作成した新しいプロジェクト）:

```text
split-payment-orchestrator/
├── app.config.yaml
├── package.json
├── .env                            ← Copy from .env.example, then fill in
└── actions/
```


## &#x200B;6. entity_idとincrement_idの比較

> **REST呼び出しでは、常に`increment_id`ではなく`entity_id` （数値データベース ID）を使用します（例：`000000042`）。**

**[!UICONTROL Commerce Admin]**&#x200B;注文URLから`entity_id`を検索：

```text
/admin/sales/order/view/order_id/42/   →   entity_id = 42
```

またはシミュレーションスクリプトから：

```bash
node scripts/simulate-split-payment.mjs show 42
```


## &#x200B;7. 100 ドルの閾値

分割された支払いUIとしきい値ガード ターゲット注文&#x200B;**$100.00以下**。 フローは次のように動作します。

* **100 ドル以下：**&#x200B;の分割支払いUIが表示されます。お客様は現金と店舗のクレジット分割を設定できます
* **$100を超える：** `CheckoutPlugin`は、支払い手順でエラーをスローします

テストするには、小計、送料、税金が&#x200B;**100** ドル以下のカートを作成します（例えば、$90未満の商品は、送料と税金がキャップの下に収まります）。

しきい値は次の場所に保存されます。

* Commerce設定：`split_payment/general/threshold` （デフォルト：`100`、`etc/config.xml`）
* App Builder環境：`PAYMENT_THRESHOLD=100` （Commerceと一致する必要があります）


## &#x200B;8. Fastly （Commerce Cloudのみ）

App Builder アクションは、REST （`/rest/V1/split-payment/orders/...`）経由でCommerceを呼び出します。 Commerce Cloud プロジェクトでIP 許可リストに加えるでFastlyを使用する場合は、App Builder ランタイムエグレス IP アドレスを許可リストに加えるする必要があります。

**確認方法：** シミュレーションスクリプトを最初に実行します（OAuth署名を使用して直接カール）。 これが機能するが、App Builder アクションが`403`を返す場合、Fastlyはリクエストをブロックしている可能性があります。

Adobeの現在のドキュメントでApp Builderの出力IP範囲を確認し、必要に応じてFastly設定に追加してください。


## 確認用チェックリスト

ビルドプロンプトを開始する前に、次の点を確認してください。

* [ ] Commerce バージョンは2.4.5以降です
* [ ]個のI/O イベントが有効（Commerce Cloud）になっていて、デプロイされています
* [ ]代金引換は、タイトルが`Cash`に正確に設定された状態で有効になっています
* [ ] ストアクレジットが有効です
* [ ] テストのお客様は、少なくとも$50 ストアクレジットを持っています
* [ ] Commerce統合が作成され、アクティブ化され、4つのOAuth値がすべて保存されます
* [ ] App Builder プロジェクトには、I/O Events サービスとCommerce イベントプロバイダーが設定されています
* [ ] `aio login`が完了し、`aio app use`で正しいワークスペースが選択されました
* [ ] Node.js 18以降がインストールされ、`aio` CLIがインストールされています
* [ ] `.env` ファイルは、[分割支払いPOC：環境変数リファレンス &#x200B;](./env-reference.md) （およびソースパッケージを使用している場合）ごとに準備されます


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

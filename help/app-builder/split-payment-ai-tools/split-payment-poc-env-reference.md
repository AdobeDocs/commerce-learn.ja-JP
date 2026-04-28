---
title: 分割支払POC：環境変数リファレンス
description: Commerce OAuth、ベース URL、支払いしきい値、およびオプションのデモ設定を、オーケストレーター、UI拡張機能、およびシミュレーション環境ファイルにマッピングする方法について説明します。
feature: App Builder, Configuration, Extensibility, Paas, REST, Security
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 115
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 1e2c7e0e6d0f2d174b88406ce3fb7c787676ecee
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# 分割支払POC：環境変数リファレンス

すべてのコンポーネントで、同じ4つのCommerce OAuth資格情報が使用されます。 **[!UICONTROL Commerce Admin]**&#x200B;で、**[!UICONTROL Integration]**&#x200B;を1つ作成してから、下の`.env` ファイルごとに4つの値を再利用してください。 （アクティベーション手順については、[分割支払いPOC：前提条件と環境セットアップ ](split-payment-poc-prerequisites-and-setup.md)を参照してください）。

## 4つのOAuth資格情報（どこでも使用）

| 変数 | どこから入手するか |
| --- | --- |
| `COMMERCE_CONSUMER_KEY` | **[!UICONTROL Commerce Admin]** > **[!UICONTROL System]** > **[!UICONTROL Integrations]** > *[統合]* |
| `COMMERCE_CONSUMER_SECRET` | 上記と同様 – 値はアクティベーション時にのみ表示されます |
| `COMMERCE_ACCESS_TOKEN` | 上記と同じ |
| `COMMERCE_ACCESS_TOKEN_SECRET` | 上記と同じ |


## App Builder orchestrator

### `split-payment-orchestrator/.env`

オーケストレーターディレクトリの`.env.example`からコピーします。 このファイルをコミットしないでください。

```dotenv
# Commerce REST base URL — no trailing slash
COMMERCE_BASE_URL=https://your-store.example.com

# OAuth 1.0a integration credentials
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=

# Must match split_payment/general/threshold in Commerce config (default: 100)
# Both Commerce and App Builder fall back to 100 if this is missing, non-numeric, or ≤ 0
PAYMENT_THRESHOLD=100

LOG_LEVEL=info

# Demo dashboard: if set, requires ?secret=<value> in URL or x-demo-secret header
# Leave empty for private staging only (anyone with the URL can list/accept orders)
DEMO_UI_SECRET=

# Optional: override the base URL used in dashboard action links (useful behind proxies)
DEMO_UI_BASE_URL=
```


## Experience Cloud UI拡張機能（commerce-checkout-starter-kit）

### `commerce-checkout-starter-kit/.env`

このコンポーネントでは、2つの資格情報セットを使用します。**[!UICONTROL Admin]** UI SDKを使用した注文リストにはIMSを使用し、承認アクションと拒否アクションにはOAuth 1.0aを使用します。

```dotenv
# IMS — used by CustomMenu/commerce-rest-api to list orders
# The Admin UI SDK provides the IMS token context; these set the Commerce base URL
COMMERCE_BASE_URL=https://your-store.example.com
OAUTH_CLIENT_ID=
OAUTH_CLIENT_SECRETS=
OAUTH_TECHNICAL_ACCOUNT_ID=
OAUTH_TECHNICAL_ACCOUNT_EMAIL=
OAUTH_SCOPES=
OAUTH_IMS_ORG_ID=
AIO_CLI_ENV=stage

# OAuth 1.0a — same four credentials, COMMERCE_INTEGRATION_ prefix
COMMERCE_INTEGRATION_BASE_URL=https://your-store.example.com
COMMERCE_INTEGRATION_CONSUMER_KEY=
COMMERCE_INTEGRATION_CONSUMER_SECRET=
COMMERCE_INTEGRATION_ACCESS_TOKEN=
COMMERCE_INTEGRATION_ACCESS_TOKEN_SECRET=
```


## シミュレーションスクリプト

### `commerce-backend-ui-1/.env.simulation`

同じディレクトリの`.env.simulation.example`からコピーします。

```dotenv
COMMERCE_BASE_URL=https://your-store.example.com
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=
```


## メモ

**`PAYMENT_THRESHOLD`** — **[!UICONTROL Commerce]** システム構成の`split_payment/general/threshold`と一致する必要があります。 値が見つからない場合、数値でない場合、または`0`以下の場合、両側のデフォルトは`100`です。 **[!UICONTROL Commerce]**&#x200B;のしきい値を変更する場合は、App Builder `.env`を一致するように更新します。

**`DEMO_UI_SECRET`** — オプションですが、localhost以外のデプロイメントでは推奨されます。 Anyone with the dashboard URL can list orders and run accept and decline if this is empty. For a real staging environment, set a shared secret.

**`COMMERCE_BASE_URL`** — Never include a trailing slash. The Commerce REST client appends `/rest/V1/` automatically.

**`AIO_CLI_ENV`** — Set to `stage` for the **[!UICONTROL Stage]** workspace. Change to `prod` when you deploy to **[!UICONTROL Production]**.


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

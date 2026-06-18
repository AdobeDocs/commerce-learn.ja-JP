---
title: 分割払いPOC – 概念実証の次のステップ
description: 分割支払POCを本番環境に移行する方法を説明します。 Experience Cloud UI、ERP フック、API メッシュ、PHP スコープ、App Builder ワークフロー、デプロイチェックリスト。
feature: App Builder, API Mesh, Extensibility, Paas, REST, Eventing
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 269
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '852'
ht-degree: 0%

---

# 分割払いPOC：概念実証の次のステップ

このチュートリアルで作成したデモダッシュボードとシミュレーションスクリプトは、意図的に大まかなものです。 そのパターンを証明するために存在しているのです。 概念実証から実稼動型App Builder開発までの実用的な道筋を説明します。


## ステップ 1 - デモダッシュボードをExperience Cloud UI拡張機能に置き換える

`demo-dashboard` web アクションは、Node.js関数内の文字列からHTMLを提供します。 動作しますが、制作パターンではありません。

適切な置き換えは、`commerce-checkout-starter-kit`の`commerce-backend-ui-1`拡張機能です。Adobe管理UI SDKを介してCommerce管理シェルに埋め込まれたReact アプリケーションです。 生成プロンプトについては、[分割支払いPOC: Experience Cloud UI拡張機能AI プロンプト ](./experience-cloud-ui-prompt.md)を参照してください。

**変更内容：**
* オペレーターは、Commerce管理シェル（共有シークレットではなくIMS認証）を介してログインします
* 注文リストでは、IMS トークンのコンテキストが使用されます。デモの秘密鍵は必要ありません
* 受け入れ/拒否アクションは、オペレーターのIMS IDにスコープされます
* UIは、Commerce管理者が既に知っているURLのCommerce管理者に埋め込まれています

**同じままの状態：**
* `payment-accept`と`payment-decline`個のApp Builder アクションは変更されません
* Commerce REST エンドポイントは変更されません
* PHP モジュールは変更されません


## ステップ 2 – 実際のERPの接続

このPoCの承認/拒否フローは、人間がボタンをクリックすることによって実行されます。 ERP、CRM、決済代行会社などの主要なツールを使用して。

**統合パターン：**
1. ERP システムが現金をキャプチャし、`POST /payment-accept` （App Builder web アクション URL）を`{ orderId: <entity_id> }`で呼び出します
2. App Builderは呼び出し（ベアラートークンまたはAPI キー – `payment-accept`に認証ミドルウェアを追加）を検証します
3. App BuilderはCommerce REST `cash-received`を呼び出します
4. Commerceの請求書と注文の出荷

PHPの変更は必要ありません。 REST サーフェスは既にそこにあります。 App Builderのアクションには、ダッシュボードボタンではなく安全な呼び出し元が必要です。

**ERP呼び出し元の認証オプション：**
* Adobe I/O Runtimeは、IMS トークンの`require-adobe-auth: true`をサポートしています（ERPでIMS トークンを取得できる場合）
* Adobe以外のシステムの場合：`payment-accept` アクションにシンプルなAPI キーのチェックを追加します（アクションのenvに保存されている秘密鍵に対してヘッダーをチェックします）


## ステップ 3 - API メッシュをブローカーレイヤーとして追加

現在、App BuilderはCommerce RESTをOAuth 1.0a資格情報で直接呼び出しています。 大規模なデプロイメントの場合、Adobe API Meshは、App BuilderとCommerceの間に管理されたゲートウェイレイヤーを提供します。

**特典：**
* 一元的な資格情報管理：API MeshはCommerceの資格情報を保持し、App BuilderはAPI Meshを呼び出します
* 変換をリクエスト — App Builder アクションを変更せずにApp Builder ペイロードをCommerce REST シェイプにマッピングします
* レートの制限とキャッシュ – 大量のイベントトラフィックからCommerceを守ります

**パターン：**
* `createCommerceClient(params, logger)`呼び出しをAPI Mesh エンドポイントへの呼び出しに置き換えます
* API Meshは、CommerceへのOAuth署名を処理します


## ステップ 4 - PHP フットプリントを削減する

現在のPHP モジュールは、処理中に残す必要がある5つの処理を処理します（[分割支払いPOC: アーキテクチャとデザインの決定](./architecture-and-decisions.md)を参照）。 Adobe CommerceのAPI サーフェスが成熟するにつれ、これらの一部が可動になる場合があります。

**将来的に移動可能：**
* ストアクレジット REST APIは進化しています。今後のバージョンでは、注文後または非アクティブなカートへのクレジットの適用をサポートする場合があります
* より多くのCommerce操作が非同期セーフになるにつれて、しきい値ガードを事前に注文されたAPI Mesh リゾルバーに移動できる場合があります

**移動可能でない（アーキテクチャの制約）:**
* CommerceがAPI ファーストの拡張ポイントを介してクリーンフックを公開しない限り、`placeOrder()`より前の見積もり操作には常にインプロセス コードが必要です
* REST エンドポイント （`/V1/split-payment/*`）は、この機能に固有です。Commerce内部サービスを呼び出すため、Commerceに存在します


## ステップ 5 - App Builderにさらにワークフローを追加する

現在のPOCでは、注文の配置をリッスンしてから、受け入れ/拒否を有効にするという1つの操作が行われます。 自然な拡張：

承認する前に&#x200B;**不正行為のスコアリング：**
`payment-orchestrator`で、現金保留を記録した後、オーケストレーション結果が最終的と見なされる前に、詐欺スコアリング APIを呼び出します。 スコアがしきい値を超えている場合は、オペレーターのアクションを待つのではなく、自動辞退します。

**通知メール：**
`payment-accept`が成功したら、現金の支払いが確定したことを通知する電子メール（Adobe Campaign、SendGrid、または任意のHTTPS API経由）をトリガーします。

**ロイヤルティポイント特典：**
現金が確認されたら、ロイヤルティ APIを呼び出してポイントを獲得します。 これは純粋なApp Builderであり、PHPは必要ありません。

**タイムアウト処理：**
スケジュールされたApp Builder アクション（`app.config.yaml`の`cron`を使用）を追加して、X日より古い`split_cash_status = 'pending'`件の注文をスキャンし、自動的に拒否します。


## ステップ 6 – 実稼動環境へのデプロイ

PoCは`Stage` ワークスペース用に設定されています。 本番環境への移行：

```bash
# Switch to production workspace
aio app use
# Select Production workspace

# Update .env for production
# (same credentials, but production Commerce base URL)

# Deploy
aio app deploy
```

実稼動準備の&#x200B;**チェックリスト：**
* [ ] `DEMO_UI_SECRET` セット （またはExperience Cloud UIに置き換えられたデモダッシュボード）
* [ 実稼動環境の] `LOG_LEVEL=warn`または`error` （`debug`ではありません）
* [ ] `PAYMENT_THRESHOLD`はCommerceの実稼動設定と一致します
* [ `.env`の] Commerce統合資格情報は、専用の実稼動統合用です（ステージング用ではありません）
* [ ] Fastly IPの許可リストに加えるがApp Builder実稼動エグレス IPで更新されました（Commerce Cloud）
* [ 実稼動ワークスペースで]個のI/O イベント登録が確認されました


## アーキテクチャ進化図

```
PoC (this tutorial)
┌─────────────────────────────────────────────────┐
│  Commerce                 App Builder            │
│  ┌─────────────┐         ┌──────────────────┐   │
│  │  PHP Module │◄────────│ payment-orchestr │   │
│  │  REST API   │────────►│ payment-accept   │   │
│  │  Checkout UI│         │ payment-decline  │   │
│  └─────────────┘         │ demo-dashboard   │   │
└─────────────────────────────────────────────────┘

Phase 2: Experience Cloud UI
┌─────────────────────────────────────────────────┐
│  Commerce                 App Builder            │
│  ┌─────────────┐         ┌──────────────────┐   │
│  │  PHP Module │◄────────│ payment-orchestr │   │
│  │  REST API   │────────►│ payment-accept   │   │
│  │  Checkout UI│         │ payment-decline  │   │
│  │             │         │ Admin UI ext.    │   │
│  └─────────────┘         └──────────────────┘   │
└─────────────────────────────────────────────────┘

Phase 3: Real ERP + API Mesh
┌──────────────────────────────────────────────────────────┐
│  ERP  ──►  App Builder  ──►  API Mesh  ──►  Commerce     │
│            (orchestrator)   (gateway)    (PHP module      │
│            (accept)                      REST surface)    │
└──────────────────────────────────────────────────────────┘
```


## 主な参考資料

* [Adobe App Builder ドキュメント](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [Adobe I/O Events for Commerce](https://developer.adobe.com/commerce/extensibility/events/){target="_blank"}
* [Commerce Checkout Starter Kit](https://github.com/adobe/commerce-checkout-starter-kit){target="_blank"}
* [Adobe管理UI SDK](https://developer.adobe.com/commerce/extensibility/admin-ui-sdk/){target="_blank"}
* [Adobe API Mesh](https://developer.adobe.com/graphql-mesh-gateway/){target="_blank"}
* [Adobe I/O Runtime（OpenWhisk）](https://developer.adobe.com/runtime/docs/){target="_blank"}



{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

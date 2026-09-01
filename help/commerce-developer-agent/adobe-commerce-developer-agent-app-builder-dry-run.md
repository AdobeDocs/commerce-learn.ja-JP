---
title: Adobe Commerce Developer Agent App Builder ドライラン
description: Adobe Commerce Developer Agentを使用して、3つのCommerce拡張性ユースケースを構築、デプロイ、テストする方法を、この実践App Builder ドライランで説明します。
feature: Extensibility, App Builder, Eventing, Configuration
topic: App Builder, Development, Integrations
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 438
last-substantial-update: 2026-08-28T00:00:00Z
source-git-commit: 92af5355fa31c1ce9e627679b0a1bb92cce0e1d8
workflow-type: tm+mt
source-wordcount: '1700'
ht-degree: 0%

---

# Adobe Commerce Developer Agent App Builder ドライラン

Adobe Commerce Developer Agent （CDA）を使用して、Commerce拡張性のユースケースを構築、デプロイ、テストするための実践的なチュートリアルです。 このドライランでは、3つのユースケース（カート数量制限webhook、高価値の注文保留、保留中の注文のイベント駆動型アーカイブ）を、ブループリントから機能テストまでカバーしています。

## Adobe Experience Managerの導入方法

### 問題とフィードバックの報告方法

ドライラン全体を通して、新しい機能の操作中に期待されるラフなエッジが発生します。 オンボーディング中に提供されたフィードバックテンプレートを使用して、Adobeプログラムの連絡先の問題を取得し、共有します。

>[!TIP]
>
> 問題を報告する場合：
>
> * `projectId`を含めます（ブラウザーのURLに表示）。
> * 必要に応じてスクリーンショットを配置します。

### 前提条件

**アカウントとアクセス**

* 少なくとも、早期アクセス IMS組織の&#x200B;**開発者**&#x200B;の役割。
* その組織内のAdobe Commerce as a Cloud Service （ACCS） インスタンスへの管理者アクセスは、**Cloud Service インスタンス**&#x200B;のexperience.adobe.comで利用できます。
* GitHub アカウント。

**ツール**

機能検証には、Edge Delivery Services（EDS）ストアフロントが必要です。 次のようなものがあります。

* Node.js 22以降
* Adobe I/O CLI: `npm install -g @adobe/aio-cli`
* AIO CLI Commerce プラグイン：`aio plugins:install https://github.com/adobe-commerce/aio-cli-plugin-commerce`

空のフォルダーにストアフロントボイラープレートをインストールし、プロンプトが表示されたらACCS インスタンスを選択します。

```bash
aio commerce extensibility app-setup -s aem-boilerplate-commerce -n storefront
```

ストアフロントを開始します。

```bash
cd storefront
npm run start
```

## Commerce Developer Agentを開く

1. **Developer Agent**&#x200B;の下のexperience.adobe.comにあるCommerce Developer Agentに移動します。
1. Early Access IMS組織の資格情報を使用してログインします。

## ユースケース 1：買い物かごの最大単位のWebhook

このユースケースでは、同期Commerce Webhookを使用して、商品を追加する前にカート数量の制限を検証します。

### ブループリントステージ

次のプロンプトを入力し、**ブループリントを生成**&#x200B;をクリックします。

```text
Add a validation webhook that runs before a product is added to the cart.

Use the Commerce webhook method observer.sales_quote_item_save_before (type before) — do not use
observer.checkout_cart_product_add_before, observer.sales_quote_add_item, or any other event.

Calculate the total by summing all quote line quantities and the quantity of the current item.
If the same SKU already exists in the quote, exclude its existing quantity to avoid double-counting.

If the total is greater than the maximum allowed, block the add and show:
"You have reached the maximum amount of items."

The maximum allowed must be configurable in Commerce Admin as max_cart_units, with default 10.

Map payload fields using name and source properties:
- name: item.qty, source: data.item.qty
- name: item.sku, source: data.item.sku
- name: quote, source: context_checkout_session.get_quote[items.qty,items.sku]

Set required: true and fallback_error_message: "You have reached the maximum amount of items."
on the webhook config.

When blocking the add, do not use exceptionOperation, because it serializes exceptionClass as class.
Instead, manually return an exception operation response whose body includes type:
{
  "op": "exception",
  "message": "You have reached the maximum amount of items.",
  "type": "\\Magento\\Framework\\GraphQl\\Exception\\GraphQlInputException"
}
```

>[!NOTE]
>
> 次のものを探します。
>
> * 要件をキャプチャしたブループリント（v1）が作成されます。
> * 実装をガイドするタスクが作成されます。

チャットボックスに詳細を入力するか、チャットボックスの上にある丸薬のいずれかをクリックして、設計図を絞り込みます（*前提条件*、*デザインギャップを見つける*&#x200B;など）。 問題がなければ、**プランを承認**&#x200B;をクリックして次に進みます。

### 現像ステージ

エージェントは現像ステージに移行し、ワークスペースのプロビジョニングを開始します。

>[!NOTE]
>
> エクスプローラーパネルで次のファイルを探します。
>
> * `app.commerce.config.ts`
> * `app.config.yaml`
> * `install.yaml`
> * `package-lock.json`
> * `package.json`

プロビジョニングが完了すると、エージェントは実装タスクのリストを表示し、構築を開始します。

>[!NOTE]
>
> 次のものを探します。
>
> * 生成されたコードは要件に一致します。
> * `Validate` ストリーミング画面に、ワークスペースの検証時の進行状況が表示されます（`aio app build`）。
> * 検証が失敗した場合、エージェントは生成されたコードを自己修正します。

コードに問題がなければ、「**統合**」タブをクリックして先に進みます。

### 統合の設定

**App Builder ワークスペースを接続または作成**

App Builder プロジェクトを作成または接続するには、画面の指示に従います。

既存のワークスペースに接続する場合は、次の要素が含まれていることを確認します。

* `Runtime` サービスが追加されました。
* 追加されたAPIは、Adobe Commerce as a Cloud Service、I/O Management API、App Builder Data Services、I/O Events、Adobe I/O Events for Adobe Commerceです。

新しいワークスペースを作成する場合は、**Adobe Commerce as a Cloud Service** APIを手動で追加します。

>[!IMPORTANT]
>
> 既存のApp Builder プロジェクトに接続したら、**詳細設定**&#x200B;を展開してワークスペース JSONを貼り付け、**ステータスを再確認**&#x200B;をクリックして、必要なすべてのAPIがインストールされていることを確認します。

**次へ**&#x200B;をクリックして続行します。

**Commerceに接続**

リストからACCS インスタンスを選択するか、**Commerce REST Base URL** フィールドにURLを入力し、**Commerce インスタンスを接続**&#x200B;をクリックします。 **次へ**&#x200B;をクリックして続行します。

**GitHubに接続**

リポジトリ URLを入力し、GitHub アプリまたは個人アクセストークンを使用して、ワークスペースをGitHub リポジトリに接続します。 **次へ**&#x200B;をクリックして続行します。

**環境変数の設定**

プロジェクトに必要な環境変数を入力します。

### デプロイ

**現像**&#x200B;をクリックして現像ステージに戻り、プロンプトフィールドに配置するようにエージェントに依頼します。

>[!NOTE]
>
> 「デプロイメントの確認」メッセージで、組織、プロジェクト、Workspace、およびランタイムの名前空間を示します。

デプロイメントを確認します。

>[!NOTE]
>
> 次を探します。
>
> * デプロイ前の検証の進行状況を示す`Validate` ストリーミング画面。
> * 検証が失敗した場合、エージェントはコードを自己修正します。
> * デプロイメントの進行状況を示す`Deploy` ストリーミング画面（`aio app deploy`）。
> * デプロイメントが失敗した場合に、エージェントがコードを自己修正します。

### App Managementでのアプリの関連付け

1. ACCS インスタンスの管理者URLに移動してログインします。
1. 左側のメニューで「**アプリ**」を選択し、**アプリ管理**&#x200B;を選択します。
1. 「**+ Associate App**」（右上）をクリックします。
1. CDAがデプロイされたプロジェクトとWorkspaceを選択し、**関連付け**&#x200B;をクリックします。

>[!NOTE]
>
> アプリケーション名とバージョン、および実装された機能（ビジネス設定、Webhook、イベントなど）を示すカードを探します。

### アプリ管理でのインストールと設定

1. アプリケーションの行で、**Install**&#x200B;をクリックし、**Close**&#x200B;をクリックします。
1. 同じ行で、**Configure**&#x200B;をクリックしてビジネス設定の値を入力し、**Close**&#x200B;をクリックします。

>[!NOTE]
>
> ブループリントで指定したすべての設定フィールドを示すフォームを探します。指定したデフォルトで事前に入力されています。

### 機能テスト

1. アプリ管理アプリの設定で、**最大カートユニット**&#x200B;を3に設定します（クイックテストには低い値を指定します）。
1. ストアフロントでは、空のカートから始めます。
1. 合計数量が3を超えるまで、製品詳細ページ（PDP）から製品を追加します。最後の追加は失敗します。
1. PDPでは、「*」が表示されます。「項目の最大数に達しました。」*
1. 制限を下回ると、追加は成功します。

>[!NOTE]
>
> 製品リストページ（PLP）から、ブロックされた追加はメッセージなしでサイレントに失敗します。これはストアフロントの動作であり、Webhookの失敗ではありません。 検証にはPDPを使用します。

## ユースケース 2：高価値の注文保留と検証コード

**ブループリント** ステージに戻って、このユースケースを開始します。

### ブループリントステージ

次のプロンプトを入力し、**ブループリントを生成**&#x200B;をクリックします。

```text
Add a Commerce event priority subscription to `plugin.sales.api.order_management.place`.

Extract `entity_id` and `grand_total` from the Commerce event payload using event `fields` in `app.commerce.config.ts`.

Important: the runtime action receives a CloudEvents-shaped payload. For Commerce eventing extracted fields,
parse them from `params.data.value`, not directly from `params.data`. The handler must use:
- `params.data.value.entity_id`
- `params.data.value.grand_total`

When `grand_total` is greater than `order_hold_threshold`:
1. Generate a verification code locally.
2. Put the order on hold with state and status `holded`.
When putting the order on hold, save the verification code using `custom_attributes`, not `extension_attributes`.
The Commerce `POST V1/orders` payload should include:
{
  "entity": {
    "entity_id": <entity_id>,
    "state": "holded",
    "status": "holded",
    "custom_attributes": [
      {
        "attribute_code": "<hold_verification_attribute>",
        "value": "<verification_code>"
      }
    ]
  }
}
3. Save the verification code via a `POST V1/orders` Commerce REST API call.

Make these configurable in Commerce Admin:
- `order_hold_threshold`, default `500`
- `hold_verification_attribute`, default `lab_verification_code`

Validate inputs before use:
- `entity_id` must be a positive integer.
- `grand_total` must be a non-negative number.
```

>[!NOTE]
>
> 次のものを探します。
>
> * 要件をキャプチャしたブループリント（v2）が作成されます。
> * 元のプランタスクは保持されます。
> * 新しい要件に対応する新しいタスクが追加されました。

必要に応じてブループリントを調整し、**計画を承認**&#x200B;をクリックして先に進みます。

### 開発、デプロイ、関連付け、インストール

ユースケース 1で使用したのと同じプロセスに従って、要件からインストールされているアプリケーションに移行します。統合を再構成する必要はありません。

>[!IMPORTANT]
>
> 既に関連付けられているアプリに変更を適用するには、**関連付けを解除**&#x200B;して&#x200B;**アプリ管理で再度関連付ける**&#x200B;必要があります。

### 機能テスト

1. App Management アプリ設定で、**注文保持しきい値（USD）**&#x200B;を50に設定します（テストカートで超えやすくなります）。
1. 注文カスタム属性が存在することを確認します（デフォルト `lab_verification_code`）。
1. 総額50 ドル以上の注文をおこないます。
1. 最大30秒待ちます（イベントは非同期です。優先度のない配信には最大59秒かかる場合があります）。
1. Commerce Admin → Sales → Ordersで、注文を開きます。 ステータスは&#x200B;**保留中** （`holded`）です。カスタム属性には、ランダムな値を持つ`lab_verification_code`が含まれます。
1. オプション：50 ドル未満の注文を最初に発注します。このハンドラーは注文を保留にしません。

## ユースケース 3：保留中の注文のイベント駆動型アーカイブ

**ブループリント** ステージに戻って、このユースケースを開始します。

### ブループリントステージ

次のプロンプトを入力し、**ブループリントを生成**&#x200B;をクリックします。

```text
When an order is saved with state holded, archive it to external storage and
record a reference that can be looked up later by order ID.

Add an event priority subscription on observer.sales_order_save_after, filtered to fire only when
state equals holded. From the event payload, extract:
- `entity_id`
- `payment.amount_ordered`
- `custom_attributes` (to read the `lab_verification_code` attribute set in Step 3)

The event handler must:
1. Persist the order details to the `held_orders` App Builder DB collection:
{
  "order_id": <entity_id>,
  "grand_total": <payment.amount_ordered>,
  "verification_code": <lab_verification_code>,
  "archived_at": <ISO timestamp>
}
2. Ensure the record can be looked up later by order ID.

The `held_orders` collection must exist before the handler runs:
- Provision persistent App Builder Database Storage in region `amer`.
- Create the collection during app installation.
- Create a unique index on `order_id` during installation.
- Drop the whole `held_orders` collection when the app is uninstalled.

Register the event handler separately from the existing cart validation webhook and high-value order hold action:
- runtime action: `order-archive/archive-held-order`
- non-web action
- `include-ims-credentials: true` on the archive action and the installation action

Follow the `commerce-app-storage` skill for DB auth, installation steps, and ext.config wiring.
Do not use custom IMS credential normalization or `Core.AuthClient.generateAccessToken`.
```

>[!NOTE]
>
> 次のものを探します。
>
> * 要件をキャプチャしたブループリント（v3）が作成されます。
> * 元のプランタスクは保持されます。
> * 新しい要件に対応する新しいタスクが追加されました。

必要に応じてブループリントを調整し、**計画を承認**&#x200B;をクリックして先に進みます。

### 開発、デプロイ、関連付け、インストール

前のユースケースで使用したのと同じプロセスに従って、要件からインストールされたアプリケーションに移行します。統合を再構成する必要はありません。

>[!IMPORTANT]
>
> 既に関連付けられているアプリに変更を適用するには、**関連付けを解除**&#x200B;して&#x200B;**アプリ管理で再度関連付ける**&#x200B;必要があります。

### 機能テスト

1. ユースケース 2のしきい値がテストに十分に低いことを確認します（アプリ設定で$50など）。
1. その基準値を超えて注文を行うと、ユースケース 2は注文を保留にします（約30秒）。
1. Adobe Developer Console → Your Project → Stage → Eventsで、保留中のアーカイブイベント（追加またはインストール時に更新）の登録を開きます。
1. 注文が保留に移行した後、その登録にイベントが配信されたことを確認します。 `order-archive/archive-held-order`にリンクされたCommerce イベントのイベント トレースまたはモニタリングを使用します。

>[!NOTE]
>
> イベントは非同期です。注文が保留されてから最大30～59秒後に許可されます。

## トラブルシューティング

CDAによって生成されたアプリケーションが期待どおりに動作しない場合、またはエラーが発生している場合は、現像ステージからトラブルシューティングを実行するようにエージェントに依頼します。

>[!NOTE]
>
> CDAは、その外で起こるステップを可視化できません。 アソシエイト、インストール、設定、機能テストはすべて、CDAではなく、Commerce管理、アプリ管理、ストアフロントで実行されます。 いずれかの分野で問題が発生した場合、担当者はその問題を把握できないため、次の機能が不足しています。
>
> * 実行した内容と場所（例：「アプリ管理でインストール」をクリック）。
> * 何が起こるだろうと期待していたか。
> * 何が起こったのか。
> * 画面に表示されている正確なエラーテキストまたはメッセージ。
> * ブラウザーコンソール、またはAdobe Developer ConsoleのApp Builder ログとEvent Registration Debug Tracesからの関連エラー。

レポートが具体的であればあるほど、担当者は問題をより的確に診断できます。

## オプションの手順

**コードをダウンロード**

お気に入りのIDEで継続的に改良または編集するには、現像ステージエクスプローラーのツールバーにあるダウンロードアイコンをクリックして、CDAによって生成されたコードをダウンロードします。 保存先フォルダーを選択し、**保存**&#x200B;をクリックしてから、ワークスペースパッケージを解凍します。

>[!NOTE]
>
> 次を探します。
>
> * 現像ステージエクスプローラーに表示されるすべてのファイルは、解凍されたフォルダーに存在します。
> * `aio app build`でプロジェクトを構築する際に「コンパイル」エラーが発生しません。

CDAが使用するのと同じエージェントスキルを使用するには、プロジェクトフォルダーにインストールします。

```bash
npx skills add adobe/aio-commerce-sdk --skill commerce-app-init -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-eventing -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-webhooks -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-business-config -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-storage -y && \
npx skills add adobe/skills --skill appbuilder-project-init -y
```

次に、IDEまたはCLIを起動し、プロンプトの表示を開始します。

**ファイルまたはリンクを介してコンテキストを添付**

ブループリントまたは現像ステージで直接プロンプトを表示する代わりに、テキストファイルまたはリンクを使用してコンテキストを添付できます。

1. チャットボックスの添付ファイルのアイコンをクリックします。
1. 「**ファイルを追加**」をクリックしてローカルテキストファイルをアップロードするか、URLを入力して「**リンクを追加**」をクリックして、リモートファイルを介してコンテキストを追加します。
1. 「**完了**」をクリックし、プロンプトを入力してエージェントを移動します。

>[!NOTE]
>
> 添付ファイルのコンテキストを次の順番に組み込む担当者を探します。

## 既知の問題と回避策

**ブループリントステージでタスクが生成されない**

ブロックを解除して続行するには、エージェントを移動してタスクを生成します。

**GitHubへのプッシュとプルのボタンが機能しません**

代わりに、現像ステージからプロジェクト ZIP ファイルをダウンロードします。

{{$include /help/_includes/commerce-developer-agent-related-links.md}}

## 関連資料

* [Commerce Developer Agentの概要](https://developer.adobe.com/commerce/extensibility/developer-agent/)
* [Commerce Developer Agentの概要](https://developer.adobe.com/commerce/extensibility/developer-agent/getting-started)
* [Commerce Developer Agentのプロンプトのヒント](https://developer.adobe.com/commerce/extensibility/developer-agent/prompting)
* [Commerce Developer Agentのサポートとフィードバック](https://developer.adobe.com/commerce/extensibility/developer-agent/support)

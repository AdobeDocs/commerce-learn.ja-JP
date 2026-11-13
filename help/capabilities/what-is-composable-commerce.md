---
title: AdobeによるコンポーザブルCommerce
description: Adobeのプラットフォームオプション、Edge Delivery Servicesストアフロント、API Mesh、App Builder、Commerce開発MCP ツールによる、コンポーザブルコマースについて説明します。
feature: API Mesh, App Builder, Saas
topic: Architecture, Commerce, Development, Headless, Integrations, Performance, Personalization
role: Admin, Developer, User
level: Intermediate
doc-type: Tutorial
duration: 307
last-substantial-update: 2026-11-13
jira: KT-15730
exl-id: 4d811a2f-8488-4de7-babd-449aced42e3a
TQID: 'https://experienceleague.adobe.com/NG-US7zLBgzV425mheo3oQ9Z6gnzAr6aRCLNjHVU0aM'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
    internal-label: Security
  - id: c32adafa-ed01-4b31-997e-2413013911b0
    internal-label: Integrations
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
  - id: d3b92bef-63fa-5031-a925-d04d9362d616
    internal-label: Saas
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
subfeature_v2:
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
    internal-label: Customer experience
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
    internal-label: Security
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
    internal-label: Personalization
  - id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
    internal-label: Privacy
autotag-review: '2026-11-13T20:04:10.194Z'
source-git-commit: 3a52f4de8c63f08418f4fa44833a7bf7eb90175b
workflow-type: tm+mt
source-wordcount: '1311'
ht-degree: 0%
---
# コンポーザブルコマース

コンポーザブルコマースとは、APIを通じてコマース機能を統合するアーキテクチャ型のアプローチです。 各アプリケーションの能力をすべて1つのアプリケーションで構築する必要はなく、ストアフロントのエクスペリエンス、カタログ管理、製品の検索、チェックアウトなどのビジネスニーズに合わせてコンポーネントを選択して統合できます。

ヘッドレスコマースでは、ストアフロントのプレゼンテーション層とコマースのバックエンドを分離します。 構成可能なコマース基盤を利用すれば、その分離をさらに強化し、連携する機能や個別に統合する機能を選択できるようになります。 ヘッドレスストアフロントでは、バックエンドコンポーネントを個別に置き換えることができません。

Adobeは、コマースプラットフォームのオプション、ストアフロントテクノロジー、統合および開発ツールを通じて、このアプローチをサポートしています。 Adobe Commerce as a Cloud Service（ACCS）、Adobe Commerce Optimizer（ACO）、Edge Delivery Services、API Mesh、App Builder、Commerce開発のMCPは、それぞれ異なる目的を兼ね備えています。 これらは、交換可能な製品や、必要な組み合わせスタックではありません。

## メリットとトレードオフ

構成可能なアプローチでは、次のことが可能になります。

* **顧客体験を適応させる：** ストアフロントコンポーネントを選択し、ビジネス要件を満たすサービスを統合します。
* **段階的な最新化：**&#x200B;他のシステムとそのデータを保持しながら、選択した機能を置き換えます。
* **個別に拡張と拡張を行う：**&#x200B;統合と拡張機能を個別に拡張し、変更する際にコマースアプリケーション全体をリリースする必要がないように設計します。
* **コマース機能を再利用：**&#x200B;あらゆるビジネス機能を再構築するのではなく、サポートされているプラットフォーム機能とAPIを使用します。

これらのメリットは、実装によって異なります。 技術的な成熟度、配信コスト、ライセンス、セキュリティ、データプライバシーを評価する。 サービスをまたいで、API契約、データ同期、認証、パフォーマンス、エラーの所有権を定義できます。 よりモジュール化されたアーキテクチャは、統合や運用の複雑さも向上させます。

## Adobe Commerceのアーキテクチャオプション

まず、マネージドコマース基盤が必要か、既存のトランザクションバックエンドを中心に顧客体験を最新化すべきかを判断しましょう。

ビデオの比較については、[Adobeの利用可能なコマースオプション &#x200B;](./adobes-available-commerce-options.md)を参照してください。

### Adobe Commerce as a Cloud Service

Adobe Commerce as a Cloud Serviceは、マネージド型のSaaS コマースプラットフォームです。 Adobeは、中核となるアプリケーション、インフラストラクチャ、アップデートを管理します。 そのアーキテクチャには、Commerce Foundation、ヘッドレスストアフロント能力、マーチャンダイジングサービス、開発者ツールなどが含まれています。

このオプションは、サポートされている拡張ポイントを使用してマネージドコマース基盤を評価する場合に使用します。 カスタムビジネスロジックの場合は、既存のPHP拡張機能を変更せずにデプロイできるものと仮定するのではなく、サポートされるプロセス外の拡張性を計画してください。 ACCSはLuma ストアフロントをサポートしていません。該当する場合は、実装計画にストアフロントの移行を含めてください。

[Adobe Commerce as a Cloud Serviceの概要](https://experienceleague.adobe.com/ja/docs/commerce/cloud-service/overview)を参照してください。

### Adobe Commerce Optimizer

Adobe Commerce Optimizerは、カタログ管理、商品の発見、検索、レコメンデーション、ストアフロント体験をサポートすると同時に、既存のコマースバックエンドを維持します。 カタログデータは外部のソースから取得する可能性があり、サードパーティシステムがカートとチェックアウトのライフサイクルを担当し続ける可能性があります。

顧客向けの近代化が目標であり、トランザクションバックエンドの置き換えが必要ない場合は、このオプションを使用します。 カタログの取り込みやカートとチェックアウトの統合を明示的に計画。 ACCSとACOは個別のアーキテクチャオプションであり、必要な組み合わせスタックではありません。 実装のサービスを選択する前に、現在の統合サポートを確認してください。

[Adobe Commerce Optimizerの概要](https://experienceleague.adobe.com/ja/docs/commerce/optimizer/overview)を参照してください。

### 既存のAdobe Commerceの展開

Adobe Commerceのクラウドインフラストラクチャとオンプレミスのデプロイメントでは、ヘッドレスストアフロントとサポートされているAPI ベースの統合を段階的に導入することもできます。 デプロイメントに応じて、既存のストアフロントを維持するか、サポート対象のヘッドレスストアフロントを導入することができます。 既存のコマース機能を統合し続けることで、統合作業を削減しながら、選択したサービスを個別に進化させることができます。

機能の可用性、拡張機能の互換性、ライセンス、設定の要件は、製品やバージョンによって異なります。 アーキテクチャを選択する前に、[Adobe Commerce プラットフォームとサービス &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce/information-center/explore-platforms-and-services/platform-and-services)を比較します。

## Edge Delivery Servicesを活用したCommerce Storefront

Edge Delivery Servicesを活用したCommerce Storefrontは、ストアフロントのエクスペリエンスとコンテンツ配信のレイヤーを提供します。 ヘッドレスアーキテクチャにより、コマース基盤とは独立してフロントエンドを開発できます。 ドキュメントベースのオーサリングとビジュアル編集は、コンテンツ更新をサポートし、ストアフロントコンポーネントは、ショッピング機能にコマース APIを使用します。

ストアフロントのインターフェイスを、カタログデータ、価格設定、カート運用、チェックアウト、注文を担当するシステムと区別します。 Edge Delivery Servicesがトランザクションコマースのバックエンドに取って代わることはありません。 ストアフロントを導入する前に、選択したバックエンドでサポートされるサービスと統合パスを特定します。

[Commerce Storefrontのドキュメント &#x200B;](https://experienceleague.adobe.com/en/tools/commerce-storefront/)を参照してください。

## 統合と拡張性

API MeshとApp Builderを利用すれば、コマース基盤のアプリケーションにあらゆるカスタマイズを組み込む必要がなくなり、コマース機能を連携および拡張できます。

### API メッシュ

ADOBE DEVELOPER APP BUILDER用API Meshは、複数のAdobe、プライベート、サードパーティのAPI ソースを組み合わせて、クエリ可能なGraphQL エンドポイントにします。 フロントエンド体験のためのデータを作成し、既存のサービスへのアクセスを調整するために利用できます。

API コンポジションでは、API コントラクト、認証、エラー処理、トランザクションビジネスロジックの所有権を定義する必要はありません。 統合設計において、それらの責任を明確にしましょう。

[API メッシュの概要](https://developer.adobe.com/graphql-mesh-gateway/)を参照してください。

### Adobe App Builder

Adobe App Builderは、Adobe ソリューションを拡張し、Adobe I/O Runtimeを含むAdobe インフラストラクチャ上で実行されるアプリケーションのプラットフォームを提供します。 CRM、ERP、PIMなどのシステムとの統合を利用して、すぐにプロセスを開始できます。

使用するCommerce製品でサポートされている拡張ポイント、イベント、webhookを選択します。 コアコマースリリースのライフサイクルとは別に、アプリケーションを設計してテストします。

[App Builderの概要](https://developer.adobe.com/app-builder/docs/intro_and_overview/)を参照してください。

## Commerceの開発MCPとスキル

Commerceの開発MCPとスキルは、App BuilderアプリケーションのAI支援による開発とストアフロントのカスタマイズをサポートします。 また、ACCSに移行する際に、既存のAdobe Commerce PHP拡張機能をApp Builder アプリケーションに移行する際にも役立ちます。

モデルコンテキストプロトコル（MCP）サーバーは、互換性のあるAI コーディングクライアントを開発ツールに接続します。 Adobeのツールには、Commerce Developer MCP Server、App Builder MCP Server、Dropins AI Toolsに加えて、Commerce固有のスキルが含まれています。

Commerceの開発MCPは、ストアフロント配信、カート、チェックアウト、注文処理サービスではなく、開発ツールです。 これらのツールは、あらゆるCommerce製品に対応するわけではありません。 生成された変更を確認し、ターゲット環境に照らしてテストします。

[Commerce開発MCPとスキル &#x200B;](https://developer.adobe.com/commerce/extensibility/developer-agent/coding-tools)を参照してください。

## コンポーザブル実装の計画

1. ビジネス目標、顧客体験、セキュリティ要件、チームが利用できるスキルなどを定義します。
2. コマースプラットフォームを選択し、維持または置換する既存システムを特定します。
3. サポートされているストアフロントを選択し、プレゼンテーション、マーチャンダイジング、トランザクションサービスの境界を定義します。
4. カタログ情報、価格、在庫、顧客、カート、チェックアウト、注文について、データの所有権とAPI契約を設定します。
5. 統合をまたいで認証、データ同期、キャッシュ、モニタリング、エラー処理を計画できます。
6. 各リリースの前に、機能、パフォーマンス、セキュリティを段階的に導入し、検証します。

コンポーザブル実装は、コンポーネントがビジネス要件を満たし、確実に連携したときに成功します。 アーキテクチャをよりモジュール化するためだけにサービスを追加するのではなく、必要な機能を選択します。

## 関連リソース

アーキテクチャの計画とコマース統合の構築に役立つチュートリアル、ビデオ、実装ガイド、プレイリストをご確認ください。

### Commerceのプラットフォームとストアフロント

* [Adobe Commerce as a Cloud Serviceのアーキテクチャ](../commerce-as-a-cloud-service/architecture.md)
* [コンポーザブルストアフロントとモジュール式アーキテクチャ](../commerce-as-a-cloud-service/product-overview/composable-storefront-and-modular-architecture.md)
* [Adobe Commerce Optimizerの概要](../adobe-commerce-optimizer/overview.md)
* [コンポーザブルカタログデータモデルが存在する理由](../adobe-commerce-optimizer/composable-catalog-data-model/why-ccdm-exists.md)
* [Edge Delivery Servicesによるショッピング体験](../edge-delivery-services/shopper-experience.md)

### 統合と開発

* [App Builderによるプロセス外の拡張性](../app-builder/foundations/introduction-to-app-builder.md)
* [API Meshの基本を学ぶ](../api-mesh/getting-started-api-mesh.md)
* [複数のソースを持つAPI メッシュの作成](../api-mesh/graphql-multiple-source.md)
* [AIとMCP ツールを使用してカスタム配送料を構築](../app-builder/custom-shipping-rates-ai-tools/ai-tools-starter-kit.md)

### アーキテクチャおよび開発ドキュメント

* [Adobe Commerce as a Cloud Serviceへの移行](https://experienceleague.adobe.com/ja/docs/commerce/cloud-service/migration/overview)
* [Adobe Commerce Optimizerとの連携](https://experienceleague.adobe.com/ja/docs/commerce/optimizer/integrations-overview)
* [Commerce Storefront開発を始める](https://experienceleague.adobe.com/en/tools/commerce-storefront/get-started/)
* [Commerce開発MCPとスキルのベストプラクティス](https://developer.adobe.com/commerce/extensibility/developer-agent/best-practices)

### プレイリスト

* [Adobe App BuilderとAdobe Commerceを使用したAPI Meshの基本を学ぶ](https://experienceleague.adobe.com/ja/playlists/commerce-get-started-app-builder-and-api-mesh)

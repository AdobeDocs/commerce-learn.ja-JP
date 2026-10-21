---
title: 最新のAdobe Commerceの実装計画
description: このウェビナーでは、Adobe Commerceを中心とした最新のコマースシステムの導入計画について解説します。調査段階、サービス、グローバルリファレンスアーキテクチャ、フロントエンド開発プラクティス、ベストプラクティスについて解説します。
feature: API Mesh, App Builder, Best Practices, Extensibility, Integration
topic: Architecture, Commerce, Integrations, Performance
old-role: Architect, Data Architect, Leader
role: Developer, Leader
level: Beginner, Intermediate
doc-type: Technical Video
duration: 3624
last-substantial-update: 2024-05-07T00:00:00.000Z
jira: KT-15465
exl-id: a8e1cd14-e9ff-46ba-87ea-c1cba3b4f72f
TQID: 'https://experienceleague.adobe.com/QO48-EUvrWmkJHTmaboYTDtFKX9UjtBabUgkwGEZxJM'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: d1e21356-0064-4f48-9089-16e3f0dbd2a6
    internal-label: Storefront
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
  - id: cdfd3bc1-dc23-5cf0-b965-d3c0c55cde67
    internal-label: Best Practices
  - id: 72863f3c-9d27-5dda-afe1-d9f934b1fba0
    internal-label: Extensibility
  - id: cc04bd17-78d5-5120-8c3d-1b8a57e49280
    internal-label: Integration
  - id: c4f010fa-1478-4300-a88d-706fbc036a7a
    internal-label: APIs and SDKs
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
subfeature_v2:
  - id: f8ddfd3b-6194-46e8-a176-0e918039be56
    internal-label: Cloud architecture
  - id: ce84ce08-883f-4337-ae83-6bb1855ca732
    internal-label: API Mesh
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
role_v2:
  - id: f8a45b24-4be7-4f1b-909b-60d06b483a20
    internal-label: Leader
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
    internal-label: Troubleshooting
source-git-commit: 7516911fc14bc80f327310b99a00f9cbff7c65c7
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 0%
---
# 最新のAdobe Commerceの実装計画

このウェビナーでは、カタログサービス、カスタマーアーキテクチャの理解、Edge Delivery Servicesなど、Adobe Commerceに関連するさまざまなトピックを包括的に解説します。
Adobe Commerce Catalog as a Serviceのライブ検索と商品レコメンデーション、カスタマーアーキテクチャの理解におけるAPIとデータフローの分析、Edge Delivery Servicesの高性能と低遅延など、各サービスの新しい機能や既存の機能とメリットについて説明します。 さらに、顧客とのやり取りにおいて、標準文書や質問リストを持つことの重要性、効率的なプロジェクト構築のためのグローバル参照アーキテクチャ（GRA）の概念についても言及しています。

## オーディエンス

* 開発チーム、マネージャー、リード開発者、テクニカルアーキテクト
* アップグレード、移行、新しいコマース機能としてAdobe Commerceを導入するチーム

## ビデオコンテンツ

* Experience Leagueには、ライブサーチや商品レコメンデーションなどのカタログサービスが含まれており、Adobe Commerceに関するガイドも用意されています。
* 顧客アーキテクチャを把握するには、API、データフロー、典型的な製品などを分析し、顧客要件とコマース機能をマッピングする必要があります。
* 顧客のアーキテクチャを把握し、プロジェクトをスムーズに導入するには、一連の標準文書と質問リストを作成することが不可欠です。
自動分析ツールは移行プロジェクトに使用できますが、その他のケースでは、手作業によるドキュメントのレビューと質問が必要になります。
* 最近Adobe Commerce向けにリリースされたEdge Delivery Servicesは、低遅延で簡素化されたオーサリング体験を実現する高性能なストアフロントソリューションを提供します。
* Edge Delivery Servicesはパフォーマンスに重点を置いており、Lighthouse スコア、SEO、web サイト全体の速度を大幅に向上させることができます。
Adobe Commerceの概要と、最新の実装の計画、構築、維持に関する4部構成のシリーズです。
* 効率的なプロジェクト構築のためのグローバル参照アーキテクチャ（GRA）の概念の概要。
* 発見フェーズにおける、役割、主要ドキュメント、プロジェクトコンポーネントの理解の重要性に関する議論。
* 価格やデータ接続など、さまざまなサービスのメリットや機能に関する説明。
* スケーラブルなコマース基盤の必要性と、コア製品をカスタマイズすることなくアプリケーションを拡張するためのサービスの使用を重視します。
* プロジェクトで最も困難な部分から始め、再利用可能なコンポーネントにGRAを使用することを検討するなど、タスクの適切な計画、整理、優先順位付けをおこなう重要性。

>[!VIDEO](https://video.tv.adobe.com/v/3428987?learn=on)

{{$include /help/_includes/enablement-series-related-links.md}}

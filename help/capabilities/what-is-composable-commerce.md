---
title: Adobeで構成可能なCommerceを作成する方法
description: コンポーザブルコマースについて学び、API ファーストのアプローチを優先し、モジュール式でサービス指向のアーキテクチャを実装する。
feature: App Builder, Saas
topic: Architecture, Commerce, Development, Headless, Integrations, Performance, Personalization
role: Admin, Developer, User
level: Beginner, Intermediate
doc-type: Tutorial
duration: 323
last-substantial-update: 2024-07-6
jira: KT-15730
exl-id: 4d811a2f-8488-4de7-babd-449aced42e3a
TQID: https://experienceleague.adobe.com/NG-US7zLBgzV425mheo3oQ9Z6gnzAr6aRCLNjHVU0aM
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: c32adafa-ed01-4b31-997e-2413013911b0
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
  - id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 1305
ht-degree: 0%

---

# コンポーザブルコマース

コンポーザブルコマースとは、フロントエンドのプレゼンテーション層とバックエンドのコマース機能を分離する、e コマースにおけるアーキテクチャアプローチのことです。 &#x200B;最適なコンポーネントやモジュールを選択して組み合わせ、カスタマイズされたソリューションを構築できます。 このアプローチでは、従来のモノリシック e コマースプラットフォームを、より小さく独立したサービスやマイクロサービスに分割し、それらを統合します。 コンポーザブルコマースには、柔軟性、スケーラビリティ、カスタマイズ、俊敏性、他のシステムやテクノロジーとの統合を容易にする機能などの利点があります。

Adobe Commerceは、コンポーザブルコマースの導入と実装において、マーチャントをサポートする多くの機能とツールを提供します。 Adobe Commerceは、構成可能なコマース手法と、ハイブリッドヘッドレスおよびヘッドレス以外のフロントエンド体験を提供します。 Adobeでは、プロセス外での拡張性を念頭に置いて、複数のサービスを統合するAPI Meshと、カスタムマイクロサービスを作成するAdobe App Builderを提供しています。

## コンポーザブルコマースが重要な理由

コンポーザブルコマースについて理解することは、さまざまな理由から、e コマース業界に携わる企業や個人にとって重要です。 柔軟性とカスタマイズ、スケーラビリティと俊敏性、統合機能、将来性、開発者への能力強化を提供します。 コンポーザブルコマースとは、企業がe コマースプラットフォームを適応および進化させ、事業を拡大および拡大することを可能にします。 さらに、他のシステムやテクノロジーと統合し、進化し続ける顧客の期待に対応し、開発者の専門知識を活用できることも、大きなメリットをもたらします。

進化するe コマース環境で適切な意思決定をおこない、柔軟性、拡張性、カスタマイズ、俊敏性、統合機能などの利点を活用できます。 さらに、コンポーザブルコマースについて理解することは、ビジネスの成長、カスタマイズ、俊敏性とイノベーション、コスト効率、統合と柔軟性、将来を見据えた対応において重要です。 これにより、企業は新機能に迅速に適応し、変化する市場状況に対応して、高度にカスタマイズされたソリューションを構築できます。 さらに、迅速なイノベーション、開発および保守コストの削減、サードパーティのサービスやシステムとの統合などのメリットもあります。

全体として、構成可能なコマースについて把握することで、情報にもとづいた意思決定、e コマースアーキテクチャと戦略の最適化、デジタル市場における成長と成功の促進を実現できます。

## コンポーザブルコマースのメリット

企業は、コンポーザブルコマースから、いくつかの利点を得ることができます。

**柔軟性とカスタマイズ：** コンポーザブルコマースを使用すると、企業は最適なコンポーネントまたはマイクロサービスを選択して組み合わせ、特定のニーズを満たすカスタマイズされたe コマースソリューションを作成できます。 これにより、企業はプラットフォームをカスタマイズして、独自の顧客体験を提供し、専門的な機能を導入して、市場での差別化を図ることができます。

**スケーラビリティと俊敏性：** コンポーザブルアーキテクチャにより、企業はe コマースプラットフォームのさまざまなコンポーネントを個別に拡張できます。 つまり、リソースを割り当て、需要に基づいて特定の機能を拡張することで、最適なパフォーマンスとコスト効率を実現できます。 また、コンポーザブルコマースはアジャイルな開発を可能にし、様々なコンポーネントを同時に使用して作業し、必要に応じて変更を個別にデプロイできるため、市場投入までの時間を短縮できます。

**統合機能：** コンポーザブルコマースは、サードパーティのシステム、サービス、テクノロジーとのシームレスな統合を促進します。 支払いゲートウェイ、ERP システム、CRM システム、マーケティングオートメーションプラットフォームなど、さまざまなツールをe コマースプラットフォームに簡単に接続できます。 この統合機能により、企業は最適なソリューションを活用し、業務効率と顧客体験を向上させる統合エコシステムを構築できます。

**将来性：** コンポーザブルコマースは、テクノロジーや市場のトレンドの変化に合わせてe コマースプラットフォームを柔軟に適応させ、進化させることができます。 企業は、コンポーネントの追加や交換、新しいテクノロジーの統合を簡単におこない、競合他社の一歩先を行くことができます。 この将来を見据えた機能により、企業は継続的にイノベーションを起こし、進化する顧客の期待に応えることができます。

**開発者の能力強化：** コンポーザブルコマースは、様々なテクノロジー、言語、フレームワークを柔軟に操作できるようにすることで、開発者を支援します。 開発者は、特定のコンポーネントやマイクロサービスに集中することができ、専門性と専門知識が可能になります。 これにより、開発者の生産性を向上し、開発サイクルを高速化して、最新のツールやテクノロジーを活用できるようになります。

包括的で構成可能なコマース能力を活用すれば、変化するビジネスニーズに適応し、優れた顧客体験を提供して、デジタル市場における成長と成功を促進できる、柔軟性と拡張性に優れ、カスタマイズ性の高いe コマース基盤を構築できます。

## Adobe Commerceが備える、コンポーザブルコマースの機能

Adobe Commerceには、コンポーザブルコマースの導入と実装においてマーチャントをサポートするいくつかの機能があります。

**コンポーザブルCommerce手法：** Adobe Commerceは、コンポーザブルコマース手法を提供し、マーチャントがコンポーザブルアーキテクチャの原則を理解して実装できるように導きます。 この手法は、複雑さ、技術的成熟度、プロジェクトの規模などの要因を考慮しながら、柔軟性、拡張性、カスタマイズの利点を活用するのに役立ちます。

**豊富な機能：** Adobe Commerceは、GraphQLAPIを通じてアクセスできる包括的な機能セットを提供します。 この豊富な機能により、コマーススタックに必要なベンダーの数を最小限に抑え、市場投入までの時間を短縮できます。 コマーススタックの進化に合わせて、追加のサードパーティサービスや機能を構成、統合する柔軟性を維持しながら、より迅速にローンチできるようになります。

**ハイブリッドフロントエンドエクスペリエンス：** Adobe Commerceは、同じエコシステム内で、ヘッドレスとヘッドレス以外のフロントエンドエクスペリエンスの両方をサポートします。 こうした柔軟性により、特定のフロントエンドのユースケースごとに最適なアーキテクチャアプローチを選択できます。 システム全体を同時に移行することなく、ヘッドレスおよびコンポーザブルアーキテクチャに段階的に移行することができます。

**API Mesh:** Adobe CommerceのAPI Meshは、複数のマイクロサービス、サードパーティ製ツール、アプリケーションを、フロントエンド開発者向けの統合API エンドポイントに簡単に統合します。 これにより、開発者は、複数のデータソースを単一のGraphQLエンドポイントに組み合わせて、複雑さを軽減し、新しい機能やエクスペリエンスの開発とメンテナンスを合理化できます。

**Adobe App Builder:** Adobe App Builderは、マーチャントがカスタムマイクロサービスを作成し、カスタムエクスペリエンスを構築し、Adobe ソリューションを拡張できるようにするサーバーレス拡張性プラットフォームです。 With App Builder, merchants can build secure and scalable apps that extend Adobe Commerce&#39;s native functionality and integrate with third-party solutions. This eliminates the need for merchants to build and maintain their own infrastructure for customizations and microservices, reducing complexity and lowering the total cost of ownership.

These capabilities provided by Adobe Commerce simplify the adoption and implementation of composable commerce, enabling merchants to leverage the benefits of flexibility, scalability, customization, and integration capabilities while reducing complexity and development effort.

## まとめ

コンポーザブルコマースとは、フロントエンドのプレゼンテーション層とバックエンドのコマース機能を分離する、e コマースにおけるアーキテクチャアプローチのことです。 Here are the key lessons learned about composable commerce:

**Architecture:** Composable commerce follows a modular and decoupled architecture, allowing businesses to select and combine the best components or microservices to create a customized solution. This architecture provides flexibility, scalability, and agility.

**Benefits:** Composable commerce offers several benefits, including flexibility and customization, scalability and agility, integration capabilities, future-proofing, and developer empowerment. It enables businesses to adapt, scale, integrate, innovate, and leverage developer expertise.

**Considerations:** When considering composable commerce, factors such as complexity, internal technical maturity, project size and structure, customization vs. standardization, total cost of ownership, and security and data privacy should be carefully evaluated.

**Adobe Commerce:** Adobe Commerce provides capabilities and tools to support merchants in adopting and implementing composable commerce. These include a composable commerce methodology, feature-rich functionality, hybrid front-end experiences, API Mesh for integration, and Adobe App Builder for custom microservices.

**Business Impact:** Composable commerce empowers businesses to create a highly flexible, scalable, and customizable e-commerce platform. It enables them to deliver unique customer experiences, scale based on demand, integrate with other systems, future-proof their operations, and leverage developer expertise.

Understanding composable commerce is crucial for businesses in the e-commerce industry to stay competitive, adapt to changing market conditions, and deliver exceptional customer experiences. By embracing composable commerce, companies can unlock the benefits of flexibility, scalability, customization, agility, and integration capabilities, driving growth and success in the digital marketplace.

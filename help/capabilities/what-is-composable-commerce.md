---
title: Adobeが構成可能なCommerceを作成する方法
description: 構成可能なコマース、API ファーストのアプローチの優先順位付け、モジュール型でサービス指向のアーキテクチャの実装について説明します。
feature: App Builder, Saas
topic: Architecture, Commerce, Development, Headless, Integrations, Performance, Personalization
role: Admin, Architect, User
level: Beginner, Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-06-27T00:00:00Z
jira: KT-15730
exl-id: 4d811a2f-8488-4de7-babd-449aced42e3a
source-git-commit: d578c066f3e51827694c8bf85aa2324035a8b07b
workflow-type: tm+mt
source-wordcount: '1257'
ht-degree: 0%

---

# 構成可能なコマース

コンポーザブルコマースは、フロントエンドプレゼンテーションレイヤーをバックエンドコマース機能から分離する、e コマースのアーキテクチャアプローチです。&#x200B; 企業は、最適なコンポーネントやモジュールを選択して組み合わせ、カスタマイズされたソリューションを作成できます。 このアプローチには、従来のモノリシック e コマースプラットフォームを、一緒に構成できる小型の独立したサービスまたはマイクロサービスに分割することが含まれます。 構成可能なコマースには、柔軟性、スケーラビリティ、カスタマイズ、俊敏性、他のシステムやテクノロジーとの統合を容易にする機能などのメリットがあります。

Adobe Commerceは、構成可能なコマースの採用と実装において、マーチャントをサポートする多くの機能とツールを提供しています。 Adobe Commerceは、構成可能なコマース手法と、ハイブリッドヘッドレスおよび非ヘッドレスのフロントエンドエクスペリエンスを提供します。 プロセス外の拡張性を念頭に置いて、Adobeは、複数のサービスを統合するための API メッシュと、カスタムマイクロサービスを作成するためのAdobeApp Builderを提供します。

## 構成可能なコマースが重要な理由

構成可能なコマースを理解することは、e コマース業界に携わる企業や個人にとって重要です。 柔軟性とカスタマイズ、スケーラビリティと俊敏性、統合機能、将来のプルーフ、開発者のエンパワーメントを提供します。 構成可能なコマースを使用すると、企業は e コマースプラットフォームを適応させて発展させ、運用を拡張して成長させることができます。 他のシステムやテクノロジーと統合する機能、顧客の期待の変化に対応する機能、開発者の専門知識を活用する機能など、さらに優れたメリットがいくつかあります。

企業が e コマースの進化する状況をナビゲートし、情報に基づいた意思決定を行い、柔軟性、スケーラビリティ、カスタマイズ、俊敏性、統合機能のメリットを活用するのに役立ちます。 さらに、構成可能なコマースについて知ることは、ビジネスの成長、カスタマイズ、俊敏性とイノベーション、コスト効率、統合と柔軟性、将来の保証にとって重要です。 これにより、企業は新機能に迅速に適応し、変化する市場条件に対応し、高度にカスタマイズされたソリューションを作成できます。 イノベーションを迅速に行える、開発コストとメンテナンスコストを削減できる、サードパーティのサービスやシステムと統合できるなどのメリットが増えています。

全体的に、構成可能なコマースを理解することで、企業は十分な情報に基づいた意思決定を行い、e コマースアーキテクチャと戦略を最適化し、デジタル市場での成長と成功を推進できるようになります。

## 構成可能なコマースから企業がメリットを得る方法

企業は、構成可能なコマースからメリットを得る方法はいくつかあります。

**柔軟性とカスタマイズ：** 構成可能なコマースを使用すると、企業は最適なコンポーネントやマイクロサービスを選択して組み合わせ、特定のニーズを満たすカスタマイズされた e コマースソリューションを作成できます。 これにより、企業は独自の顧客体験を提供するためにプラットフォームを調整し、特別な機能を実装し、市場で差別化することができます。

**スケーラビリティと俊敏性：** 構成可能なアーキテクチャにより、企業は e コマースプラットフォームの様々なコンポーネントを個別に拡張できます。 つまり、リソースを割り当て、必要に応じて特定の機能を拡張できるので、最適なパフォーマンスとコスト効率を確保できます。 また、構成可能なコマースはアジャイルな開発手法も可能で、チームが異なるコンポーネントを同時に作業し、変更を独立してデプロイできるので、市場投入までの時間が短縮されます。

**統合機能：** 構成可能なコマースは、サードパーティのシステム、サービス、テクノロジーとのシームレスな統合を容易にします。 企業は、e コマースプラットフォームを、支払いゲートウェイ、ERP システム、CRM システム、マーケティング自動化プラットフォームなどのさまざまなツールと簡単に接続できます。 この統合機能により、企業は優れたソリューションを活用し、運用効率と顧客体験を向上させる統合エコシステムを作成できます。

**Future-Proof:** 構成可能なコマースは、テクノロジーや市場のトレンドの変化に合わせて e コマースプラットフォームを適応させ、発展させる柔軟性を企業に提供します。 これにより、企業はコンポーネントの追加や交換、新しいテクノロジーの統合、競合他社との差別化を簡単に行うことができます。 この将来性を保証する機能により、企業は継続的にイノベーションを起こし、進化する顧客の期待に応えることができます。

**開発者の能力強化：** 構成可能なコマースは、様々なテクノロジー、言語、フレームワークを扱う柔軟性を開発者に提供することで、開発者を支援します。 開発者は特定のコンポーネントやマイクロサービスに集中でき、専門性と専門知識を実現できます。 これにより、開発者の生産性が向上し、開発サイクルが短縮され、最新のツールとテクノロジーを活用できるようになります。

全体的に、構成可能なコマースにより、企業は、変化するビジネス・ニーズに適応し、卓越した顧客体験を提供し、デジタル市場での成長と成功を推進できる、柔軟性が高く、拡張性が高く、カスタマイズ可能な e コマース・プラットフォームを作成できます。

## Adobe Commerceには、構成可能なコマースにどのような機能があります

Adobe Commerceには、構成可能なコマースの採用と実装においてマーチャントをサポートする機能がいくつか用意されています。

**コンポーザブル Commerceの手法：** Adobe Commerceは、コンポーザブル アーキテクチャの原則をマーチャントが理解し、実装できるよう導く、コンポーザブル コマースの手法を提供します。 この方法は、企業が複雑さ、技術的な成熟度、プロジェクトのサイズなどの要因を考慮しながら、柔軟性、スケーラビリティ、カスタマイズのメリットを活用するのに役立ちます。

**豊富な機能：** Adobe Commerceは、GraphKLAPI を通じてアクセスできる包括的な機能セットを提供します。 この豊富な機能により、マーチャントはコマーススタックで必要なベンダーの数を最小限に抑え、市場投入までの時間の課題を軽減できます。 これにより、企業はコマーススタックの進化に合わせて追加のサードパーティのサービスや機能を構成および統合する柔軟性を維持しながら、より迅速に立ち上げることができます。

**ハイブリッドフロントエンドエクスペリエンス：** Adobe Commerceは、同じエコシステム内で、ヘッドレスと非ヘッドレスフロントエンドエクスペリエンスの両方をサポートします。 この柔軟性により、マーチャントは、特定のフロントエンドユースケースごとに最も適したアーキテクチャアプローチを選択できます。 システム全体を同時に移行する必要なく、ヘッドレスで構成可能なアーキテクチャに段階的に移行できます。

**API メッシュ：** Adobe Commerceの API メッシュを使用すると、複数のマイクロサービス、サードパーティ製ツール、アプリケーションを、フロントエンド開発者向けの統合 API エンドポイントに簡単に統合できます。 これにより、開発者は複数のデータソースを 1 つのGraphQL エンドポイントに組み合わせて、複雑さを軽減し、新しい機能やエクスペリエンスの開発およびメンテナンスを合理化できます。

**App BuilderのAdobe:** AdobeApp Builderは、マーチャントがカスタムマイクロサービスの作成、カスタムエクスペリエンスの構築、Adobeソリューションの拡張を行えるようにするサーバーレス拡張プラットフォームです。 App Builderを使用すると、マーチャントは、Adobe Commerceのネイティブ機能を拡張し、サードパーティソリューションと統合する、安全で拡張性の高いアプリを作成できます。 これにより、カスタマイズやマイクロサービス用に独自のインフラストラクチャを構築して維持する必要がなくなり、複雑さが軽減され、総所有コストが削減されます。

Adobe Commerceが提供するこれらの機能により、構成可能なコマースの導入と導入が容易になり、マーチャントは複雑さと開発作業を軽減しながら、柔軟性、拡張性、カスタマイズ、統合の機能のメリットを活用できます。

## 最終思考

コンポーザブルコマースは、フロントエンドプレゼンテーションレイヤーをバックエンドコマース機能から分離する、e コマースのアーキテクチャアプローチです。 構成可能なコマースに関して学んだ主な教訓を次に示します。

**アーキテクチャ：** 構成可能なコマースは、モジュール型で分離されたアーキテクチャに従うため、企業は最適なコンポーネントやマイクロサービスを選択して組み合わせ、カスタマイズされたソリューションを作成できます。 このアーキテクチャは、柔軟性、拡張性、俊敏性を提供します。

**メリット：** 構成可能なコマースには、柔軟性とカスタマイズ、スケーラビリティと俊敏性、統合機能、将来性の証明、開発者の能力強化など、いくつかのメリットがあります。 これにより、企業は開発者の専門知識を適応、拡張、統合、革新、活用できます。

**検討事項：** 構成可能なコマースを検討する際には、複雑さ、内部の技術的な成熟度、プロジェクトのサイズと構造、カスタマイズと標準化、総所有コスト、セキュリティとデータプライバシーなどの要因を慎重に評価する必要があります。

**Adobe Commerce:** Adobe Commerceは、構成可能なコマースの採用と実装において、マーチャントをサポートする機能とツールを提供します。 これには、構成可能なコマース手法、豊富な機能、ハイブリッドフロントエンドエクスペリエンス、統合の API メッシュ、カスタムマイクロサービスのAdobeApp Builderが含まれます。

**ビジネスへの影響：** 構成可能なコマースは、企業が非常に柔軟でスケーラブルかつカスタマイズ可能な e コマースプラットフォームを作成できるようにします。 これにより、独自の顧客体験を提供し、需要に基づいて拡張し、他のシステムと統合し、運用を将来にわたって実証し、開発者の専門知識を活用することができます。

構成可能なコマースを理解することは、e コマース業界の企業が競争力を維持し、変化する市場状況に適応し、優れた顧客体験を提供するために不可欠です。 構成可能なコマースを採用することで、企業は柔軟性、拡張性、カスタマイズ、俊敏性、統合機能のメリットを引き出し、デジタル市場での成長と成功を促進できます。

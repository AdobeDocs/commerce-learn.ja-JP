---
title: Adobe Commerce Launch、ローンチ後、継続的なモニタリングとトラブルシューティング
description: web サイトを準備し、立ち上げるためのベストプラクティスとヒントを説明します。 適切なステージング環境の選択、サポートチケットの関連情報の提供、サポートチームと同じ監視ツールを使用したコミュニケーションの改善について説明します。
feature: Best Practices, Cloud
topic: Architecture, Commerce, Performance
old-role: Architect, Data Architect, Leader
role: Developer, Leader
level: Beginner, Intermediate
doc-type: Technical Video
duration: 3576
last-substantial-update: 2024-05-07T00:00:00.000Z
jira: KT-15467
exl-id: e3748ec2-b449-4cc2-927d-e7ba4f4dbaf8
TQID: 'https://experienceleague.adobe.com/sSK6X9LrKA5LUgdELTzfyNZu8RP9juyYGDdGQfPni4w'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
    internal-label: Security
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
  - id: cdfd3bc1-dc23-5cf0-b965-d3c0c55cde67
    internal-label: Best Practices
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
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
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
    internal-label: Security
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 0%
---
# Adobe Commerce Cloudを起動、監視、トラブルシューティング

このウェビナーでは、web サイトを準備し、立ち上げるためのベストプラクティスとヒントをいくつか紹介します。 適切なステージング環境を選択し、サポートチケットに正確な詳細を提供することで、サポートチームとの効果的なコミュニケーションを促進することの重要性を強調しています。 より優れたコラボレーションと問題解決のために、New Relicなどのサポートチームと同じモニタリングツールを使用することをお勧めします。 決済代行会社などの周辺システムを徹底的にテストし、ローンチ中に適切に機能することを確認することが重要です。 また、このドキュメントでは、スムーズで成功したローンチを確実に行うために、パフォーマンスのボトルネックやキャッシュウォーミングなどの潜在的な運用上の問題を予測し、計画する必要があることも強調しています。 共有責任モデルを理解することは、もう1つの重要な側面です。データベースの復元やアプリケーションコードのセキュリティなど、特定のタスクがweb サイト所有者の責任であることが明確になります。 これらのベストプラクティスに従うことで、web サイトの立ち上げを合理化し、潜在的な問題に積極的に対処して、立ち上げを成功させることができます

## オーディエンス

* 開発チーム、マネージャー、リード開発者、テクニカルアーキテクト
* アップグレード、移行、新しいコマース機能としてAdobe Commerceを導入するチーム

## ビデオコンテンツ

* 適切なステージング環境を選択し、チケットに関連する詳細情報を提供します。
* サポートチームと同じモニタリングツール（New Relic）を使用して、コミュニケーションと問題解決を向上させる。
* 問題を説明する際に、スクリーンショットやNew Relicのリンクを提供することで、サポートがより効果的に理解し、トラブルシューティングを行えるようになります。
* 共有された責任モデル、特にデータベースの復元とセキュリティに関する理解。
* 決済代行会社などの周辺システムをテストし、カットオーバー時に注文IDが適切に処理されるようにします。
* パフォーマンスのボトルネックやキャッシュウォーミングの必要性など、生産中の潜在的な問題を予測して計画します。


>[!VIDEO](https://video.tv.adobe.com/v/3428990?learn=on)

{{$include /help/_includes/enablement-series-related-links.md}}

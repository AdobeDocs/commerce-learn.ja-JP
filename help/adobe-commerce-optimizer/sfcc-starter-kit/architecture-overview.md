---
title: Salesforce Commerce Cloud Connector Architecture
description: Salesforce Commerce Cloud Connector Starter Kitが、App Builder ランタイムアクションと差分エクスポートを使用してカタログをAdobe Commerce Optimizerと同期する方法について説明します。
feature: App Builder,Saas
topic: Administration,Commerce,Integrations
role: Developer
level: Beginner
doc-type: Technical Video
duration: 288
last-substantial-update: 2025-10-20T00:00:00.000Z
jira: KT-19014
exl-id: 1e0edcbb-5619-45c2-b06d-9133f23a634f
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: d3b92bef-63fa-5031-a925-d04d9362d616
    internal-label: Saas
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
subfeature_v2:
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: 7516911fc14bc80f327310b99a00f9cbff7c65c7
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 0%
---
# Salesforce Commerce Cloud スターターキットアーキテクチャ

Salesforce Commerce Cloud（SFCC）とAdobe App Builderを統合するCommerce Optimizer Connector Starter Kitのアーキテクチャと機能について説明します。 スターターキットは、Adobe Commerce OptimizerがEdge Delivery ストアフロントのカタログ同期を効率化するために使用します。 SFCCのカスタムカートリッジが差分エクスポートファイルを介してカタログの変更を検出し、カスタム APIを介してそれらを公開する方法について説明します。 これらの変更は、完全同期と差分同期、メタデータ更新、製品固有の同期を実行するために、同期と非同期の両方のApp Builder ランタイムアクションによって消費されます。 また、ストアフロントの正確性を確保するための検証ツールも備えており、App Builderの状態管理を使用して同期ステータスを追跡し、競合を防止します。

## この動画は誰のためのものでしょうか？

* Commerce Solution Architect
* テクニカルマーケティングエンジニア
* e コマースプラットフォーム管理者

## ビデオコンテンツ

* カスタム SFCC カートリッジとAPIにより、差分エクスポートによってカタログの変更を検出できるため、Adobe App Builderとの効率的なデータ同期が可能です。
* App Builder ランタイムアクション フル同期と差分同期、検証、状態追跡を管理して、Commerce Optimizerの更新を正確かつスムーズに行うことができます。

>[!VIDEO](https://video.tv.adobe.com/v/3476052?captions=jpn&learn=on)


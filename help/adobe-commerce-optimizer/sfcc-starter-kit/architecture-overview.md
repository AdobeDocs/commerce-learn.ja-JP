---
title: Salesforce Commerce Cloud Connectorのアーキテクチャの概要
description: Adobe Commerce Optimizerを活用したSalesforce Commerce Cloudのアーキテクチャについてご紹介します。
feature: App Builder,Saas
topic: Administration,Commerce,Integrations
old-role: Architect, Developer
role: Developer
level: Beginner
doc-type: Technical Video
duration: 288
last-substantial-update: 2025-10-20T00:00:00Z
jira: KT-19014
exl-id: 1e0edcbb-5619-45c2-b06d-9133f23a634f
source-git-commit: b859664f02cf6eac99a551e5f58dff34ca55e37a
workflow-type: tm+mt
source-wordcount: '190'
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

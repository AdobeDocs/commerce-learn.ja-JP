---
title: Salesforce Commerce Cloud Connector アプリのアーキテクチャの概要
description: Adobe Commerce Optimizerを使用したSalesforce Commerce Cloudのアーキテクチャについて説明します。
feature: App Builder,Saas
topic: Administration,Commerce,Integrations
role: Architect, Developer
level: Beginner
doc-type: Technical Video
duration: 0
last-substantial-update: 2025-10-20T00:00:00Z
jira: KT-19014
source-git-commit: 54a1a8e62e86f8ae3456bb41a1b0450134f26b71
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---


# Salesforce Commerce Cloud スターターキットアーキテクチャについて説明します

Adobe App Builder（Salesforce Commerce Cloud）を統合するCommerce Optimizer コネクタスターターキットのアーキテクチャと機能について説明します。 このスターターキットは、Adobe Commerce OptimizerでEdge Delivery ストアフロントのカタログ同期を効率化するために使用されます。 SFCC のカスタムカートリッジがデルタ書き出しファイルを使用してカタログの変更を検出し、カスタム API を使用してそれらを公開する方法を説明します。 これらの変更内容は、App Builder実行時のアクション（同期と非同期の両方）で利用され、完全な同期と差分同期、メタデータの更新、製品固有の同期を実行します。 また、ストアフロントの正確性を確保するための検証ツールも含まれており、App Builderのステート管理を使用して同期ステータスを追跡し、競合を防ぎます。

## このビデオの目的は誰ですか。

* Commerce ソリューションアーキテクト
* テクニカルマーケティングエンジニア
* e コマースプラットフォーム管理者

## ビデオコンテンツ

* カスタム SFCC カートリッジおよび API は、差分エクスポートによってカタログの変更を検出し、Adobe App Builderとの効率的なデータ同期を可能にします。
* App Builder ランタイムアクションは、Commerce Optimizerに対する完全な同期、差分の検証、状態のトラッキングを管理し、競合のない正確な更新を確実に行います。

>[!VIDEO](https://video.tv.adobe.com/v/3476052?captions=jpn&learn=on)

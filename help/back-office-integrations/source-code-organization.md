---
title: Commerce Integration Starter Kitの主なフォルダーと自動化スクリプトについて説明します
description: Commerce Integration starter kitでのソースコードの整理について説明します。​
landing-page-description: Commerce Integration Starter KitでのSource Code Organizationの調査
kt: 15868
doc-type: video
duration: 534
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: 678f4d2b-c57e-4afb-a535-1048a88bc3b1
source-git-commit: b859664f02cf6eac99a551e5f58dff34ca55e37a
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%

---

# Adobe Starter KitのSource コードの整理

Adobe Commerce Integration スターターキット内のソースコードの整理について説明します&#x200B; `actions`や`scripts`などの主要なフォルダーとそれぞれのコンテンツを強調表示して、プロジェクトの構造を確認します&#x200B; 「アクション」フォルダーには、`ingestion`や`webhook`などのサブフォルダーが含まれており、イベントの処理と追跡に必要なコードが含まれています。 また、`starter-kit-info` フォルダーと`scripts` フォルダーについても学習します。 `scripts` フォルダーは、プロジェクト内でのイベント設定とプロバイダー設定を合理化する`commerce-event-subscribe`や`onboarding`などの自動化スクリプトに焦点を当てています。
&#x200B;
ソースコード構造の背後にあるロジックを調べ、各エンティティフォルダーの`commerce`および`external` フォルダーが異なるシステムから発生するイベントをどのように処理するかを詳しく説明します。 このビデオでは、適切なイベントハンドラーのランタイムアクションにイベントをディスパッチする際の`consumer` フォルダーの役割について説明し、シームレスな処理を実現します。 このビデオでは、失敗したイベントを効果的に処理するためのランタイムアクションに実装された再試行メカニズムについても説明し&#x200B;す。Adobe Commerce Integration starter kitのソースコードの整理と機能を理解し、イベント処理、自動化スクリプト、設定設定に関する貴重なインサイトを提供します。

## オーディエンス

* ソースコードがどのように`actions`や`scripts`などの主要フォルダーに整理されているかを理解したい開発者。
* `actions` フォルダーには、`ingestion`や` webhook`などのサブフォルダーが含まれており、イベントの処理やデプロイメントのトラッキングに必要なコードが含まれています。
* `actions`、`customer`、`order`、`product`などのエンティティのフォルダーを含む`stock` フォルダーについて学習したい開発者。

## ビデオコンテンツ

* セッション中の`actions`と`scripts`のフォルダーに重点を置いて、4つの主要フォルダー（`test`、`utils`、`actions`、および`scripts`）を理解して&#x200B;ださい。
* `actions` フォルダーと、`ingestion`や`webhook`などの重要なサブフォルダーが含まれている方法について説明します。
* `actions` フォルダーと、`customer`、`order`、`product`、`stock`などのエンティティに固有のフォルダーがある理由を確認します。各フォルダーには、`commerce`および`external` フォルダーに構造化されたランタイムアクションが含まれており、Commerceおよびサードパーティシステムからのイベントを効果的に管理できま&#x200B;。
* Adobeがスターターキットに基づいてプロジェクトのデプロイをトラッキングするために使用するランタイムアクションを含む`starter-kit-info` フォルダー内のコードを変更しない&#x200B;とが重要であることを説明します。
* イベント設定、プロバイダー設定、CommerceのAdobe I/O Events モジュールの設定を自動化する`scripts`や`commerce-event-subscribe`などの自動化スクリプトを含む`onboarding` フォルダーについて説明&#x200B;ます。

>[!VIDEO](https://video.tv.adobe.com/v/3431691?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

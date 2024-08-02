---
title: 主要なフォルダーと自動化スクリプトを含むCommerce統合スターターキットについて説明します
description: Commerce統合スターターキットでのソースコードの編成について説明します。​
landing-page-description: Commerce統合スターターキットでのSource コード組織の調査
kt: 15868
doc-type: video
duration: 420
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: f0c6e9262a2bf2de3144255de1fc78d6972b6d33
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%

---

# AdobeスターターキットのSource コード組織

Adobe Commerce統合スターターキット内のソースコード組織について説明します&#x200B; プロジェクトの構造を探索し、`actions` や `scripts` などの主要なフォルダーとそれぞれの内容を強調表示します&#x200B; 「アクション」フォルダーには `ingestion` や `webhook` などのサブフォルダーが含まれており、イベントの処理と追跡に不可欠なコードが含まれています。 また、`starter-kit-info` フォルダーと `scripts` フォルダーについても説明します。 `scripts` フォルダーでは、`commerce-event-subscribe` や `onboarding` などの自動化スクリプトを重点的に使用して、プロジェクト内のイベント設定やプロバイダー設定を効率化します。
&#x200B;
ソースコード構造の背後にあるロジックを探索し、各エンティティフォルダーの `commerce` フォルダーおよび `external` フォルダーで様々なシステムからのイベントがどのように処理されるかを詳しく説明します。 このビデオでは、イベントを適切なイベントハンドラー実行時アクションにディスパッチし、シームレスな処理を確実に行う際の `consumer` フォルダーの役割について説明します。 このビデオでは、失敗したイベントを効果的に処理するためにランタイムアクションで実装されている再試行メカニズムについても説明します。&#x200B;Adobe Commerce統合スターターキットのソースコードの編成と機能について説明し、イベント処理、自動スクリプト、設定の設定に関する貴重なインサイトを提供します。

## オーディエンス

* ソースコードが `actions` や `scripts` などの主要なフォルダーにどのように整理されているかを理解したい開発者。
* イベントの処理とデプロイメントのトラッキングに不可欠なコードを含む `ingestion` や ` webhook` などのサブフォルダーを含む `actions` フォルダーについて説明します。
* `customer`、`order`、`product`、`stock` などのエンティティのフォルダーが含まれている `actions` フォルダーについて学習したい開発者。

## ビデオコンテンツ

* セッション中の `actions` フォルダーと `scripts` フォルダーに焦点を当てて、4 つのメインフォルダー（`actions`、`scripts`、`test`、`utils`）を理解します。&#x200B;
* `actions` フォルダーと、`ingestion` や `webhook` などの重要なサブフォルダーがどのように含まれているかについて説明します。
* `actions` フォルダーと、`customer`、`order`、`product`、`stock` などのエンティティに固有のフォルダーがある理由を調べます。各フォルダーには、Commerceやサードパーティシステムからのイベントを効果的に管理するための `commerce` フォルダーと `external` フォルダーに構造化されたランタイムアクションが含まれています。&#x200B;
* スターターキットに基づいてプロジェクトのデプロイメントを追跡するためにAdobeで使用されるランタイムアクションを含む、`starter-kit-info` フォルダーのコードを変更しない重要性を説明します。&#x200B;
* イベント設定、プロバイダー設定、CommerceのAdobe I/Oイベントモジュールの設定を自動化する `commerce-event-subscribe` や `onboarding` などの自動化スクリプトを含む `scripts` フォルダーについて説明します。&#x200B;

  >[!VIDEO](https://video.tv.adobe.com/v/3431691?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

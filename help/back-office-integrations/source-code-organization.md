---
title: Commerce スターターキットでのSource コードの整理
description: アクションやスクリプト、自動化スクリプト、イベント処理などの主要フォルダーを含む、Commerce Integration starter kitのソースコードの整理について説明します。
doc-type: Technical Video
duration: 534
last-substantial-update: 2024-07-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Developer
level: Intermediate
jira: KT-15868
exl-id: 678f4d2b-c57e-4afb-a535-1048a88bc3b1
TQID: https://experienceleague.adobe.com/P6-sK18TcpC91YXJcXohIvzmii3N66ZKh3nZha-RYQY
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2:
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: 9568f37b026d0e659e8092282cb923c7ecde58ac
workflow-type: tm+mt
source-wordcount: 252
ht-degree: 0%

---

# Adobe Starter KitのSource コードの整理

Adobe Commerce Integration スターターキット内のソースコードの整理について説明します。&#x200B;`actions`や`scripts`などのキーフォルダーと、それぞれの内容を強調表示して、プロジェクトの構造を説明します。&#x200B;「actions」フォルダーには、`ingestion`や`webhook`などのサブフォルダーが含まれており、イベントの処理とトラッキングに必要なコードが含まれています。また、`starter-kit-info`および`scripts` フォルダーについても学習します。`scripts` フォルダーは、プロジェクト内でのイベント設定とプロバイダー設定を合理化する`commerce-event-subscribe`や`onboarding`などの自動化スクリプトに焦点を当てています。
&#x200B;
ソースコード構造の背後にあるロジックを探り、各エンティティフォルダーの`commerce`と`external` フォルダーが異なるシステムからのイベントをどのように処理するかを詳しく説明します。このビデオでは、適切なイベントハンドラーのランタイムアクションにイベントをディスパッチする際の`consumer` フォルダーの役割を説明し、シームレスな処理を実現しています。このビデオでは、失敗したイベントを効果的に処理するためのランタイムアクションに実装された再試行メカニズムについても説明します。Adobe Commerce Integration スターターキットのソースコードの整理と機能を理解し、イベント処理、自動化スクリプト、設定設定に関する貴重なインサイトを提供します。

## オーディエンス

* ソースコードがどのように`actions`や`scripts`などの主要フォルダーに整理されているかを理解したい開発者。
* `actions` フォルダーには、`ingestion`や` webhook`などのサブフォルダーが含まれており、イベントの処理やデプロイメントのトラッキングに必要なコードが含まれています。
* `customer`、`order`、`product`、`stock`などのエンティティのフォルダーを含む`actions` フォルダーについて学習したい開発者。

## ビデオコンテンツ

* セッション中の`actions`と`scripts`のフォルダーに重点を置いて、4つの主要フォルダー（`actions`、`scripts`、`test`、および`utils`）を理解して&#x200B;ださい。
* `actions` フォルダーと、`ingestion`や`webhook`などの重要なサブフォルダーが含まれている方法について説明します。
* `actions` フォルダーと、`customer`、`order`、`product`、`stock`などのエンティティに固有のフォルダーがある理由を確認します。各フォルダーには、`commerce`および`external` フォルダーに構造化されたランタイムアクションが含まれており、Commerceおよびサードパーティシステムからのイベントを効果的に管理できま&#x200B;。
* Adobeがスターターキットに基づいてプロジェクトのデプロイをトラッキングするために使用するランタイムアクションを含む`starter-kit-info` フォルダー内のコードを変更しない&#x200B;とが重要であることを説明します。
* イベント設定、プロバイダー設定、CommerceのAdobe I/O Events モジュールの設定を自動化する`commerce-event-subscribe`や`onboarding`などの自動化スクリプトを含む`scripts` フォルダーについて説明&#x200B;ます。

>[!VIDEO](https://video.tv.adobe.com/v/3431691?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

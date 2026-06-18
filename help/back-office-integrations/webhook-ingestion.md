---
title: 取り込みWebhookの設定、デプロイおよびカスタマイズ
description: Adobe Commerceとサードパーティのバックオフィスシステムを接続し、イベント翻訳を処理するために、取り込みWebhookを設定、デプロイ、およびカスタマイズする方法について説明します。
doc-type: Technical Video
duration: 697
last-substantial-update: 2024-07-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Developer
level: Intermediate
jira: KT-15870
exl-id: f2654873-256e-4c1b-abed-8bfbc4db3fbb
TQID: https://experienceleague.adobe.com/nUXdrsjzeD939jOjZS8ywPV3OeOaxpZCmeuveACtYrY
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 9568f37b026d0e659e8092282cb923c7ecde58ac
workflow-type: tm+mt
source-wordcount: 412
ht-degree: 0%

---

# 取り込みWebhookの設定、デプロイ、カスタマイズ

Commerceとサードパーティのバックオフィスシステムを統合するための取り込みWebhookの設定とカスタマイズについて説明します&#x200B;このビデオでは、サードパーティシステムからAdobe IO イベント APIにメッセージを適応させる一般公開されたエンドポイントを提供することで、Webhookがシステム間のイベント通信の制限に対処する方法について説明します。 このプロセスでは、`actions.config.yaml` ファイルでWebhookを設定し、`app.config.yaml` ファイルで有効にし、適切な機能を確保するためにデプロイします。

このビデオでは、サードパーティイベントを統合の購読イベントタイプと互換性のある形式に変換するためのWebhook コードを変更する手順を説明します。 この変換を容易にするための`event-mapping.json` ファイルの追加について説明し、変更を行った後にランタイムアクションを再デプロイすることの重要性を強調します。&#x200B;このビデオでは、受信イベントペイロードを検証および変換して、想定されるスキーマに合わせて変換し、処理を成功させ、お客様を作成するためのCommerce APIとの統合を確実にすることの重要性も強調しています。

## オーディエンス

* 取り込みWebhookを設定する開発者
* イベント翻訳用にコードをカスタマイズしたい方
* 認証とペイロード管理の重要性を理解したい開発者とアーキテクト

## ビデオコンテンツ

* 設定と展開：このビデオでは、`actions.config.yaml` ファイルで取り込みWebhookを設定し、`app.config.yaml` ファイルで有効にすることの重要性を強調しています。 また、Webhookが正しく機能するように変更を加えた後、プロジェクトを再デプロイする必要性も強調しています。
* 互換性のカスタマイズ：サードパーティイベントを統合の購読イベントタイプに合わせた形式に変換するには、Webhook コードをカスタマイズすることが重要です。 &#x200B;このカスタマイズにより、システム間でシームレスなコミュニケーションを実現し、イベント処理を成功させることができます。
* 認証の実装：企業は、取り込みWebhookを使用する際に不正な要求を防ぐために、ニーズに適した認証メカニズムを実装する責任があります。 このステップは、統合のセキュリティと整合性を維持するために不可欠です。
* ペイロードの検証と変換：期待されるスキーマに一致するように受信イベントペイロードを検証および変換することは、処理を成功させ、Commerce APIとの統合を行うために不可欠です。 &#x200B; フィールドを適切にトリミングおよびマッピングすることで、必要なデータを使用して効率的に統合を実行できます。

>[!VIDEO](https://video.tv.adobe.com/v/3431694?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## コードサンプル

* [カスタム取り込みWebhook](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/customize-ingestion-webhook)
* [取り込みスケジューラーの追加](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/add-ingestion-scheduler)

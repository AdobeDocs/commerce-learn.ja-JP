---
title: 取り込みWebhookの設定、デプロイおよびカスタマイズ
description: Commerceとサードパーティのバックオフィスシステム間のコミュニケーションを促進するために、取り込みWebhookを設定およびカスタマイズする方法について説明します。
landing-page-description: Commerce Integration Starter Kitを使用して、取り込みWebhookを使用してCommerceをサードパーティのバックオフィスシステムと統合する方法を説明します。
kt: 15870
doc-type: video
duration: 697
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: f2654873-256e-4c1b-abed-8bfbc4db3fbb
source-git-commit: b859664f02cf6eac99a551e5f58dff34ca55e37a
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# 取り込みWebhookの設定、デプロイ、カスタマイズ

Commerceとサードパーティのバックオフィスシステムを統合するための取り込みWebhookの設定とカスタマイズについて説明&#x200B;ます。 このビデオでは、サードパーティシステムからのメッセージをAdobe IO イベント APIに適応させる公開されているエンドポイントを提供することで、Webhookがシステム間のイベントコミュニケーションの制限に対処する方法について説明します。 このプロセスでは、`actions.config.yaml` ファイルでWebhookを設定し、`app.config.yaml` ファイルで有効にし、適切な機能を確保するためにデプロイします。

このビデオでは、サードパーティイベントを統合の購読イベントタイプと互換性のある形式に変換するためのWebhook コードを変更する手順を説明します。 この翻訳を容易にするための`event-mapping.json` ファイルの追加について説明し、変更後にランタイムアクションを再デプロイすることの重要性を強調します。&#x200B; また、このビデオでは、受信イベントペイロードを検証し、想定されるスキーマに合わせて変換することで、処理を成功させ、お客様を作成するためのCommerce APIとの統合を確保することの重要性を強調しています。

## オーディエンス

* 取り込みWebhookを設定する開発者
* イベント翻訳用にコードをカスタマイズしたい方
* 認証とペイロード管理の重要性を理解したい開発者とアーキテクト

## ビデオコンテンツ

* 設定と展開：このビデオでは、`actions.config.yaml` ファイルで取り込みWebhookを設定し、`app.config.yaml` ファイルで有効にすることの重要性を強調しています。 また、Webhookが正しく機能するように変更を加えた後、プロジェクトを再デプロイする必要性も強調しています。
* 互換性のカスタマイズ：サードパーティイベントを統合の購読イベントタイプに沿った形式に変換するには、Webhook コードをカスタマイズすることが重要です。&#x200B; このカスタマイズにより、システム間でシームレスなコミュニケーションを実現し、イベント処理を成功させることができます。
* 認証の実装：企業は、取り込みWebhookを使用する際に不正な要求を防ぐために、ニーズに適した認証メカニズムを実装する責任があります。 このステップは、統合のセキュリティと整合性を維持するために不可欠です。
* ペイロードの検証と変換：期待されるスキーマに一致するように受信イベントペイロードを検証および変換することは、Commerce APIとの処理と統合を成功させるために不可欠&#x200B;す。 フィールドを適切にトリミングおよびマッピングすることで、必要なデータを使用して効率的に統合を実行できます。

>[!VIDEO](https://video.tv.adobe.com/v/3431694?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## コードサンプル

* [&#x200B; カスタム取り込みWebhook](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/customize-ingestion-webhook)
* [取り込みスケジューラーを追加](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/add-ingestion-scheduler)

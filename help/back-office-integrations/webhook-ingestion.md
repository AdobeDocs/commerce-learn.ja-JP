---
title: 取り込み Webhook の設定、デプロイ、カスタマイズ
description: Commerceとサードパーティのバックオフィスシステムとの通信を容易にする、取り込み Webhook を設定およびカスタマイズする方法について説明します。
landing-page-description: Commerce統合スターターキットを使用して、取り込み Webhook を使用してCommerceをサードパーティのバックオフィスシステムと統合する方法を説明します。
kt: 15870
doc-type: video
duration: 593
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: f2654873-256e-4c1b-abed-8bfbc4db3fbb
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# 取り込み Webhook の設定、デプロイ、カスタマイズ

Commerceをサードパーティのバックオフィスシステムと統合するための取り込み Webhook のセットアップとカスタマイズについて説明します&#x200B; このビデオでは、サードパーティシステムからAdobe IO イベント API にメッセージを適応させる公開されたエンドポイントを提供することで、システム間のイベント通信の制限に Webhook で対処する方法を説明します。 このプロセスには、`actions.config.yaml` ファイルで Webhook を設定し、`app.config.yaml` ファイルで有効にし、適切に機能するようにデプロイすることが含まれます。

このビデオでは、サードパーティイベントを統合の購読しているイベントタイプと互換性のある形式に変換するように Webhook コードを変更する手順を説明します。 ここでは、この翻訳を容易にする `event-mapping.json` ファイルの追加について説明し、変更を加えた後にランタイムアクションを再デプロイすることの重要性を強調&#x200B;ます。 このビデオでは、受信イベントペイロードを想定されるスキーマに合わせて検証および変換し、処理を成功させて、お客様を作成するためのCommerce API と統合することの重要性についても説明します。

## オーディエンス

* 取り込み Webhook を設定する開発者
* イベント翻訳用のコードをカスタマイズしたいユーザー
* 認証とペイロード管理の重要性を理解したい開発者とアーキテクト

## ビデオコンテンツ

* 設定とデプロイメント：このビデオでは、`actions.config.yaml` ファイルで取り込み Webhook を設定し、`app.config.yaml` ファイルで有効にする際の重要性を強調しています。 また、Webhook が正しく機能するように変更を加えた後にプロジェクトを再デプロイする必要があることも強調しています。
* 互換性のためのカスタマイズ：Webhook コードをカスタマイズして、サードパーティのイベントを統合の購読しているイベントタイプに合った形式に変換することが重要です。&#x200B; このカスタマイズにより、システム間のシームレスな通信とイベント処理の成功が保証されます。
* 認証の実装：企業は、取り込み Webhook を使用する際に、権限のないリクエストを防ぐために、ニーズに適した認証メカニズムを実装する責任があります。 この手順は、統合のセキュリティと整合性を維持するために不可欠です。
* ペイロードの検証と変換：受信イベントペイロードを検証し、期待されるスキーマに一致するように変換することは、処理を成功させ、Commerce API と統合するために不可欠です。&#x200B; フィールドを適切にトリミングしてマッピングすることで、必要なデータで効率的に動作します。

>[!VIDEO](https://video.tv.adobe.com/v/3431694?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## コードサンプル

* [ カスタム取得 Webhook](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/customize-ingestion-webhook)
* [ 取り込みスケジューラーの追加 ](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/add-ingestion-scheduler)

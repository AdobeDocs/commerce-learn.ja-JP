---
title: 再試行メカニズムのネイティブ機能の使用
description: 再試行条件や視覚指標など、回復力の高いアプリケーションに対して、Adobe I/O Eventsの再試行メカニズムを活用します。
landing-page-description: Adobe I/O Eventsに組み込まれた再試行メカニズムを理解および活用して、アプリケーションのレジリエンスを強化し、イベントのアクティベーションを効果的に管理します。
kt: 15872
doc-type: video
duration: 402
audience: all
last-substantial-update: 2024-7-31
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: 412060b3-76ae-4c27-bf96-8eb2a0f0d0e8
source-git-commit: b859664f02cf6eac99a551e5f58dff34ca55e37a
workflow-type: tm+mt
source-wordcount: '342'
ht-degree: 0%

---

# アプリケーションのレジリエンスのためにAdobe I/O Eventsの再試行メカニズムを活用する

このビデオでは、Adobe I/O Eventsに組み込まれた再試行メカニズムを活用して、アプリケーションのレジリエンスを強化するための包括的なガイドの概要を説明します。 特定のHTTP応答ステータスコードがトリガーイベントの再試行をどのように示しているかを説明します。 Adobe I/O Eventsでは、再試行に対して指数関数的かつ固定的なバックオフ戦略を採用しており、間隔は1分から15分に増加しています。 このドキュメントでは、開発者コンソールに再試行インジケーターがどのように表示されるかも詳しく説明しており、警告アイコンや丸い矢印などの視覚的なキューはそれぞれ失敗したイベントと再試行されたイベントを示しています。

「コンシューマー」ランタイムアクションのコンテキスト内で再試行メカニズムがどのように機能するかを説明し、イベントが再試行されるかどうかを判断します。 正常な応答は200 ステータスコードで示され、エラー応答には「statusCode」属性を持つエラーオブジェクトが含まれます。 「consumer」ランタイムアクションは、ダウンストリームの処理結果に基づいて返されるHTTP応答コードを決定し、効率的なイベント処理と最終的に正常なアクティベーションを実現します。

## オーディエンス

* トリガーイベントが再試行する特定のHTTP レスポンスステータスコードを理解したい開発者。
* Adobe I/O Eventsが再試行に使用する指数関数的および固定的バックオフ戦略について学習したいチーム。
* 開発者コンソールのビジュアルインジケーターで、失敗したイベントと再試行されたイベントがどのように表示されるかを理解したい開発者。

## 動画コンテンツ

* Adobe I/O Eventsには、特定のHTTP応答ステータスコードにもとづいてイベントアクティベーションを自動的に再試行する、すぐに使用できる再試行メカニズムが組み込まれています。
* Adobe I/O Eventsで導入されている再試行機能には、指数関数的かつ固定的なバックオフ戦略が組み込まれています。
* 失敗したイベントの警告アイコンや、再試行されたイベントの円形矢印アイコンなど、開発者コンソールの視覚的なインジケーター。
* 「consumer」ランタイムアクションは、イベント処理に適したHTTP応答ステータスコードを決定する際に重要な役割を果たします。

>[!VIDEO](https://video.tv.adobe.com/v/3431695?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## 関連ドキュメント

* [Webhookはイベントを処理できません](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-unable-to-handle-a-specific-event-but-handles-all-other-events-gracefully)
* [Webhookを停止し、不安定とマークしました](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-down-why-is-my-event-registration-marked-as-unstable)

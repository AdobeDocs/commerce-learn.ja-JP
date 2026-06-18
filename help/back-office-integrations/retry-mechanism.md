---
title: 再試行メカニズムのネイティブ機能の使用
description: Adobe I/O Eventsの再試行メカニズムを使用して、再試行条件、バックオフ戦略、視覚指標をカバーする回復力のあるアプリケーションを構築する方法について説明します。
doc-type: Technical Video
duration: 402
last-substantial-update: 2024-07-31
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Developer
level: Intermediate
jira: KT-15872
exl-id: 412060b3-76ae-4c27-bf96-8eb2a0f0d0e8
TQID: https://experienceleague.adobe.com/hrzcmSY8cAke4LBLRtqfkP8-t6jP4KMoMc7iL3WPRng
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588
source-git-commit: 9568f37b026d0e659e8092282cb923c7ecde58ac
workflow-type: tm+mt
source-wordcount: 382
ht-degree: 0%

---

# アプリケーションのレジリエンスにAdobe I/O Events再試行メカニズムを使用する

このビデオでは、Adobe I/O Eventsに組み込まれた再試行メカニズムを活用して、アプリケーションのレジリエンスを強化するための包括的なガイドの概要を説明します。 特定のHTTP応答ステータスコードがトリガーイベントの再試行をどのように示しているかを説明します。 Adobe I/O Eventsでは、再試行に対して指数関数的かつ固定的なバックオフ戦略を採用しており、間隔は1分から15分に増加しています。 このドキュメントでは、開発者コンソールに再試行インジケーターがどのように表示されるかも詳しく説明しており、警告アイコンや丸い矢印などの視覚的なキューはそれぞれ失敗したイベントと再試行されたイベントを示しています。

「コンシューマー」ランタイムアクションのコンテキスト内で再試行メカニズムがどのように機能するかを説明し、イベントが再試行されるかどうかを判断します。 正常な応答は200 ステータスコードで示され、エラー応答には「statusCode」属性を持つエラーオブジェクトが含まれます。 「consumer」ランタイムアクションは、ダウンストリームの処理結果に基づいて返されるHTTP応答コードを決定し、効率的なイベント処理と最終的に正常なアクティベーションを実現します。

## オーディエンス

* トリガーイベントが再試行する特定のHTTP レスポンスステータスコードを理解したい開発者。
* Adobe I/O Eventsが再試行に使用する指数関数的および固定的バックオフ戦略について学習したいチーム。
* 開発者コンソールのビジュアルインジケーターで、失敗したイベントと再試行されたイベントがどのように表示されるかを理解したい開発者。

## ビデオコンテンツ

* Adobe I/O Eventsには、特定のHTTP応答ステータスコードにもとづいてイベントアクティベーションを自動的に再試行する、すぐに使用できる再試行メカニズムが組み込まれています。
* Adobe I/O Eventsで導入されている再試行機能には、指数関数的かつ固定的なバックオフ戦略が組み込まれています。
* 失敗したイベントの警告アイコンや、再試行されたイベントの円形矢印アイコンなど、開発者コンソールの視覚的なインジケーター。
* 「consumer」ランタイムアクションは、イベント処理に適したHTTP応答ステータスコードを決定する際に重要な役割を果たします。

>[!VIDEO](https://video.tv.adobe.com/v/3431695?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## 関連ドキュメント

* [Webhookがイベントを処理できません](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-unable-to-handle-a-specific-event-but-handles-all-other-events-gracefully)
* [Webhookを停止し、不安定としてマーク](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-down-why-is-my-event-registration-marked-as-unstable)

---
title: 再試行メカニズムのネイティブ機能の使用
description: 再試行条件や視覚的な指標など、回復力のあるアプリケーションには、Adobe I/Oイベントの再試行メカニズムを活用します。
landing-page-description: Adobe I/Oイベントの組み込みの再試行メカニズムを理解し、利用して、アプリケーションの回復性を高め、イベントのアクティベーションを効果的に管理します。
kt: 15872
doc-type: video
duration: 314
audience: all
last-substantial-update: 2024-7-31
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: eb548dd83e3bab7a4a1486cd2cbd88efcc060121
workflow-type: tm+mt
source-wordcount: '342'
ht-degree: 0%

---

# アプリケーションの回復性のためにAdobe I/Oイベントの再試行メカニズムを活用

このビデオでは、Adobe I/Oイベントの組み込みの再試行メカニズムを活用してアプリケーションの回復性を強化する方法に関する包括的なガイドの概要を説明します。 特定の HTTP 応答ステータスコードがトリガーイベントの再試行をどのように行うかを説明します。 Adobe I/Oイベントでは、再試行に対して、1 分から 15 分に間隔を広げる、指数関数的で固定されたバックオフ戦略を採用しています。 また、このドキュメントでは、開発者コンソールでの再試行指標の表示方法について詳しく説明します。例えば、警告アイコンや、失敗したイベントと再試行したイベントを示す円の矢印などの視覚的なキューが表示されます。

「コンシューマー」ランタイムアクションのコンテキスト内で再試行メカニズムがどのように機能するかを説明し、イベントを再試行するかどうかを判断します。 正常な応答は 200 ステータスコードで示され、エラー応答には、「statusCode」属性を持つエラーオブジェクトが含まれます。 「コンシューマー」ランタイムアクションは、ダウンストリーム処理の結果に基づいて返される HTTP 応答コードを決定し、効率的なイベント処理と最終的な成功したアクティベーションを保証します。

## オーディエンス

* トリガーイベントの再試行に使用される特定の HTTP 応答ステータスコードを理解する開発者。
* 再試行でAdobe I/Oイベントで使用される指数関数的かつ固定的なバックオフ戦略について学びたいチーム。
* 開発者コンソールの視覚的な指標が、失敗したイベントや再試行したイベントをどのように表すかを理解したい開発者。

## ビデオコンテンツ

* Adobe I/Oイベントには、標準で用意されている再試行メカニズムが組み込まれており、特定の HTTP 応答ステータスコードに基づいて、イベントのアクティベーションを自動的に再試行します。
* Adobe I/Oイベントで実装される再試行メカニズムには、指数関数的なバックオフ戦略と固定的なバックオフ戦略が含まれます。
* 失敗したイベントの警告アイコンや再試行したイベントの循環矢印アイコンなど、開発者コンソールの視覚的な指標。
* 「コンシューマー」ランタイムアクションは、イベント処理に適した HTTP 応答ステータスコードを決定する上で重要な役割を果たします。

>[!VIDEO](https://video.tv.adobe.com/v/3449074?learn=on&captions=jpn)

{{$include /help/_includes/starter-kit-related-links.md}}

## 関連ドキュメント

* [Webhook がイベントを処理できません ](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-unable-to-handle-a-specific-event-but-handles-all-other-events-gracefully)
* [Webhook がダウンし、不安定としてマークされる ](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-down-why-is-my-event-registration-marked-as-unstable)

---
title: Commerce スターターキットのラストマイル統合
description: 検証、変換、前処理、送信、後処理に拡張フックを使用したCommerceでのラストマイル統合について説明します。
doc-type: Technical Video
duration: 557
last-substantial-update: 2024-07-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Developer
level: Intermediate
jira: KT-15869
exl-id: e86e8c7b-d5d2-484d-90a2-9c5309c7ea1d
TQID: https://experienceleague.adobe.com/TCR23A98L8XrVDEQeqLQoOXKQPBQu-Wb7YnGUkBXgak
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
source-git-commit: 9568f37b026d0e659e8092282cb923c7ecde58ac
workflow-type: tm+mt
source-wordcount: 342
ht-degree: 0%

---

# Adobe Starter Kitによるラストマイル統合

Adobe Commerceとのラストマイル統合を開始する際に考慮すべき項目について説明します。サードパーティシステムとの接続を強化するための拡張性フックに焦点を当てます。 このビデオでは、検証、変換、前処理、送信、後処理などのさまざまなフックがシームレスなデータフローとシステムの同期を確保する構造化されたアプローチの概要を説明します。 各フックには、次のような明確な目的があります。

* スキーマに対する受信データの検証
* システム間でのデータオブジェクトの変換
* 関連情報を送信する前に計算を実行する
* 宛先システムへのデータの送信

ビジネスロジックの整合性を維持し、今後のフレームワークのアップグレードを促進するためには、ブロックごとに個別のJavaScriptファイルを管理することが重要です。これにより、堅牢で適応性の高い統合設定を実現できます。

注文へのコメントの追加や外部IDの保存など、データ同期後に追加のアクションを実行できるポストプロセスフックを介したポストプロセスアクティビティの重要性について説明します。 このビデオには、サードパーティシステムとの接続を合理化するために、特定のライブラリ内でAPI リクエストをカプセル化するなどのベストプラクティスが含まれています。 また、各フックの一般的なユースケースと、様々なシナリオの処理に関するガイダンスについても学習します。

## オーディエンス

* 拡張性フックの構造と機能、およびこれらのフックがサードパーティシステムとの接続をどのように強化できるかを学習したい開発者。
* 検証、変換、前処理、送信、後処理など、各拡張フックに関連する典型的なユースケースとベストプラクティスを学習して、シームレスなデータフロー、システム同期、効率的な統合設定メンテナンスを促進したい開発者&#x200B;。

## ビデオコンテンツ

* ラストマイル統合で呼び出されたアクションの構造について説明します。
* スキーマに対する受信データの検証や、特定の条件に基づいて特定のイベントのスキップなど、検証フック内の典型的なユースケースを理解&#x200B;きます。
* ソースシステムと宛先システム間でデータオブジェクトを変換する際の変換フックの役割について説明します。
* 実際のデータ送信を宛先システムに容易にするための送信フックの重要性について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3431692?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

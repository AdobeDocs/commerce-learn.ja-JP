---
title: Edge Deliveryのオートコンプリート機能でアドレスレイアウトをカスタマイズ
description: チェックアウトアドレスフォームをカスタマイズし、Google Address Lookupを統合してオートコンプリートを有効にし、入力エラーを減らし、ユーザーエクスペリエンスを向上させる方法を説明します。
feature: Best Practices, Extensibility, Storefront, Tools and External Services
topic: Architecture, Commerce, Headless
role: Developer
level: Beginner
doc-type: Technical Video
duration: 856
last-substantial-update: 2025-01-13T00:00:00.000Z
exl-id: 91535671-02ff-4611-b452-0325792ad70b
jira: KT-16729
TQID: 'https://experienceleague.adobe.com/QnipDRCT13r3KSOOZj3G8A8EoAZNET6Y823qbjDAXE8'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
  - id: cdfd3bc1-dc23-5cf0-b965-d3c0c55cde67
    internal-label: Best Practices
  - id: 72863f3c-9d27-5dda-afe1-d9f934b1fba0
    internal-label: Extensibility
  - id: d1e21356-0064-4f48-9089-16e3f0dbd2a6
    internal-label: Storefront
  - id: 125c1f49-aefd-5f34-a252-288937f95f6b
    internal-label: Marketing Tools
subfeature_v2:
  - id: 618ab558-d6ad-5352-99d6-d5702c6fdf80
    internal-label: Tools and External Services
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
    internal-label: Customer experience
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
    internal-label: Insights
source-git-commit: 7516911fc14bc80f327310b99a00f9cbff7c65c7
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%
---
# アドレスレイアウトのカスタマイズ

チェックアウト時にアドレスフォームレイアウトをカスタマイズして、すべてのフィールドの幅が50%であることを確認する方法を説明します。ただし、テキストエリアは例外です。 統合されたサードパーティ API、特にGoogle Address APIを使用して、アドレスフォームでの自動アドレス検索と検証を有効にするアプローチについて説明します。 &#x200B; ストアロケーターのカスタムページにリンクして、新しいサイドバーメニュー項目をユーザーアカウントダッシュボードに追加する方法について説明します。

## この動画は誰のためのものでしょうか？

このトランスクリプトを読むことでメリットを得られる役割は次のとおりです。

* チェックアウトプロセスとユーザーアカウントダッシュボードを改善して顧客体験を向上させる方法を理解することを目的としたE コマースマネージャー。
* フロントエンド開発者：CSS調整、サードパーティ API統合、UI コンポーネントのカスタマイズに関する実用的なインサイトを得る必要がある開発者
* UX/UI デザイナーは、デザインの変更がどのように実装され、検証されて、一貫性のあるユーザーフレンドリーなインターフェイスを実現するかを確認したいと考えています。
* 開発タスクの進捗状況を追跡し、プロジェクト目標の達成に必要な技術的ステップを理解する必要があるプロジェクトマネージャー。
* 開発プロセスを監督し、コードの実装と統合においてベストプラクティスに従うことを保証するテクニカルリード。


## ビデオコンテンツ

* チェックアウト Formsの一貫したフィールド幅を使用して、チェックアウト時にアドレスフォームをカスタマイズして、すべてのフィールドを確実に保持します。
* Google Address APIを統合することで、自動アドレス検索と補完が可能になり、ユーザー入力エラーを減らし、チェックアウトプロセスを合理化できます。
* アドレス入力フィールドの検証とエラー処理。不正確または不完全な入力に対して、ユーザーがすぐにフィードバックを受け取れるようにし、ユーザーエクスペリエンスを向上させます。
* ユーザーアカウントダッシュボードに新しいサイドバーメニュー項目を追加するユーザーアカウントダッシュボードの拡張機能。

>[!VIDEO](https://video.tv.adobe.com/v/3442787?learn=on)

## その他のドキュメント

* [ユーザーアカウント](https://experienceleague.adobe.com/developer/commerce/storefront/dropins/user-account)

---
title: Adobe Commerceのプロセス外拡張機能
description: Adobe App Builder と、それがプロセス外の拡張性の重要な側面になっている理由について説明します。
landing-page-description: App Builder の概要および Adobe Commerce の開発戦略に App Builder がどのように役立つかを説明します。
short-description: App Builder の概要および Adobe Commerce の開発戦略に App Builder がどのように役立つかを説明します。
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-16
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 94f8d82a-4a95-46ea-8eed-edf9bed5760c
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '831'
ht-degree: 5%

---

# App Builder の概要

従来、Adobe Commerce開発ではインプロセス拡張機能が使用されていました。 プロセス内モデルでは、新しいコードがアップグレード、サーバーの PHP バージョン、Commerce が使用するその他の重要なサーバーアプリケーションやサービスとの互換性を持つ必要があります。 Adobe Developer App Builder は、これらの互換性の問題を回避するために、プロセス外の拡張機能を使用します。

## Adobe Commerceの App Builder {#app-builder}

>[!VIDEO](https://video.tv.adobe.com/v/3412839?quality=12&learn=on)

Adobe Developer App Builder は、Adobeソリューションを拡張するカスタムエクスペリエンスを統合および作成するための、サーバーレスの拡張プラットフォームで、Adobe Commerceで利用できるようになりました。 App Builder を使用すると、コマースネイティブの機能を拡張し、サードパーティのソリューションと統合する、安全で拡張性の高いアプリを構築できます。 開発者は、Adobe Commerceのプロセス外の拡張機能を活用できるようになり、その拡張機能によって、即時の長期的なメリットが得られます。

App Builder は、を拡張するカスタムアプリケーションを統合および作成するための、統合されたサードパーティの拡張フレームワークを提供します。 [!DNL Adobe Commerce]. この拡張フレームワークはAdobeのインフラストラクチャ上に構築されているので、開発者はカスタムマイクロサービスを構築し、を拡張および統合できます [!DNL Adobe Commerce] を他のAdobeソリューションおよびサードパーティ統合にまたがって使用する場合。

App Builder を使用すると、 [!DNL Adobe Commerce] 様々な使用例：

* ミドルウェアの拡張性 — カスタムコネクタを構築するか、事前に構築されたAdobeのスイートを利用して、外部システムを統合アプリケーションに接続します。
* コアサービスの拡張機能 — カスタム機能とビジネスロジックを使用してデフォルトの動作を拡張し、コアアプリケーション機能を拡張します。
* ユーザーエクスペリエンスの拡張：コアエクスペリエンスを拡張して、ビジネス要件をサポートしたり、顧客固有のデジタルプロパティ、ストアフロント、バックオフィスアプリケーションを構築したりします。

Adobe Developer App Builder はクラウドベースのソリューションで、自動的に拡張できます。 また、このサービスは、地域に関係なく最高のパフォーマンスを実現するために、グローバルに配布されます。

## App Builder の詳細を学ぶ理由

Adobe Commerceは完全な SAAS 製品ではないので、開発するコードは複雑さを増し、アップグレードの問題を引き起こす可能性があります。 App Builder などのプロセス外の拡張機能を使用すると、プロセス内の方法を必要とせずに、Adobe Commerceストアに独自のカスタム機能を提供できます。

その他の利点は次のとおりです。

* 切り離された機能により、起動時間を短縮できます。
* アップグレードが容易になりました。 カスタム機能はコマースコードベースの外部にあり、アップグレード時の互換性の問題を防ぎます。
* Commerce の外部で機能とロジックを移動すると、通常はインプロセス開発方法で使用されるリソースが解放されます。

## アーキテクチャ {#architecture}

Adobe Developer App Builder は、標準のソリューションの代わりに、Adobe CommerceなどのAdobeクラウドソリューションを拡張するための、共通の、一貫性のある、標準化された開発プラットフォームを提供します。

* Adobe Developer Console は、カスタムマイクロサービスと拡張機能の開発に使用されます。 プラグインや統合の作成に必要なすべてのツールと API にアクセスしながら、プロジェクトを構築し管理します。
* カスタム拡張機能および統合を構築するためのオープンソースツール、SDK およびライブラリ。 React Spectrum(Adobeの UI ツールキット ) を使用して、すべてのAdobeアプリに共通の UI を 1 つ用意します。
* Adobeのサーバーレスプラットフォーム上のインフラストラクチャをホスティングするための I/O Runtime や、イベントベースの統合用の I/O イベントなどのサービス。 Adobeには、データとファイルの保存に関する標準のサポートも用意されています。
* Adobe Experience Cloudで、拡張機能と統合を送信して、Experience Cloud組織に公開する。システム管理者は、これらの拡張機能を確認、管理および承認できます。 公開されると、カスタムの App Builder 拡張機能とツールを他のAdobe Experience Cloudアプリと共に使用できるようになります。

次の図は、App Builder 上に構築された標準アプリケーションがこれらの機能をどのように使用するかを示しています。

![アーキテクチャ](/help/assets/app-builder/app-builder-architecture.jpeg)

App Builder のアーキテクチャについて詳しくは、 [アーキテクチャの概要](https://developer.adobe.com/app-builder/docs/guides/){target="_blank"}.

## AmazonSales Channel拡張機能 {#amazon-sales-channel-extension}

>[!IMPORTANT]
>
>AmazonSales Channel拡張機能は、まだ開発中で、正式にリリースされていません。  これらのビデオとチュートリアルでは、Adobe Developer App Builder を実際の使用例で使用する方法を説明します。

以下のチュートリアルは、App Builder 拡張機能を使用してAdobe CommerceをAmazonSales Channelに接続する方法を示しています。

* [技術概要 App Builder](../app-builder/app-builder-technical-overview.md)
* [拡張フレームワーク](../app-builder/extensibility-framework-commerce-eventing.md)
* [機能デモ App Builder](../app-builder/app-builder-functional-demonstration.md)

## App Builder の概要 {#additional-resources}

初期の設定を含む、合成可能なコマース戦略の概要については、次のブログ投稿を参照してください。

[App Builder がコマースプラットフォームに対するビジネスの俊敏性の向上に役立つ理由](https://business.adobe.com/blog/how-to/how-app-builder-helps-you-implement-a-composable-commerce-strategy){target="_blank"}

App Builder の使用を開始する際に役立つように、Adobeは次のドキュメントを作成しました。

* [App Builder はじめに](https://developer.adobe.com/app-builder/docs/getting_started/){target="_blank"}

## ドキュメントを使用して学習を続ける {#appbuilder-documentation}

App Builder には、開発者向けのビデオおよびドキュメントが用意されています。この中には、独自のカスタムアプリケーションの開発に役立つガイドやリファレンスドキュメントが含まれています。

* [App Builder ドキュメント](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [App Builder ビデオ](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o){target="_blank"}

## サンプルアプリケーションの 1 つを試してみます。 {#appbuilder-codesamples}

開発を開始する準備はできていますか？ 次のリンクには、使い始めるのに役立つサンプルアプリケーションが含まれています。

* [Adobe Developer Web サイトの App Builder コードラボ](https://developer.adobe.com/app-builder/docs/resources/){target="_blank"}

## サポート {#support}

開発者サポートリクエストの場合、 [Experience Leagueフォーラム](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly){target="_blank"} 助けを求めて

{{$include /help/_includes/app-builder-related-links.md}}

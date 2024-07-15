---
title: Adobe Commerceのプロセス外の拡張性
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
source-wordcount: '796'
ht-degree: 5%

---

# App Builderについて

これまで、Adobe Commerce開発ではプロセス内拡張機能を使用してきました。 インプロセスモデルでは、アップグレード、サーバーの PHP バージョン、およびCommerceが使用するその他の多くの基本的なサーバーアプリケーションおよびサービスと互換性を持つ新しいコードが必要です。 Adobe Developer App Builderでは、このような互換性の問題を回避するために、プロセス外の拡張機能を使用しています。

## Adobe CommerceのApp Builder {#app-builder}

>[!VIDEO](https://video.tv.adobe.com/v/3412839?quality=12&learn=on)

Adobe Developer App Builderは、カスタムエクスペリエンスを統合および作成してAdobeソリューションを拡張するためのサーバーレス拡張プラットフォームであり、Adobe Commerceで利用できるようになりました。 App Builderを使用すると、Commerceのネイティブ機能を拡張し、サードパーティソリューションと統合する、安全で拡張性の高いアプリを作成できます。 開発者は、Adobe Commerceのプロセス外の拡張機能を利用できるようになり、すぐに長期的なメリットが得られます。

App Builderは、サードパーティを拡張するカスタムアプリケーションを統合および作成するための統一サードパーティ拡張フレームワーク [!DNL Adobe Commerce] 提供します。 この拡張フレームワークはAdobeのインフラストラクチャに基づいて構築されているため、デベロッパーはカスタムマイクロサービスを構築し、他のAdobeソリューションやサードパーティ統合に [!DNL Adobe Commerce] たって拡張および統合を行うことができます。

App Builderを使用すると、次のような様々なユースケースで [!DNL Adobe Commerce] を拡張できます。

* ミドルウェア拡張 – カスタムコネクタを構築するか、事前に構築された統合のスイートを利用して、外部システムとAdobeアプリケーションを接続します。
* コアサービス拡張 – カスタム機能およびビジネスロジックを使用してデフォルトの動作を拡張することで、コアアプリケーション機能を拡張します。
* ユーザーエクスペリエンス拡張 – コアエクスペリエンスを拡張してビジネス要件をサポートするか、顧客固有のデジタルプロパティ、ストアフロントおよびバックオフィスアプリケーションを構築します。

Adobe Developer App Builderはクラウドベースのソリューションです。つまり、自動で拡張できます。 また、このサービスは世界中に分散しているため、地理的な場所に関係なく最高のパフォーマンスを実現できます。

## App Builderの詳細を確認する必要がある理由

Adobe Commerceは完全な SAAS 製品ではないので、開発するコードは複雑になり、問題をアップグレードする可能性があります。 App Builderなどのプロセス外の拡張機能を使用すると、プロセス内の手段を必要とせずに、Adobe Commerce ストアに独自のカスタム機能を提供できます。

その他の利点は次のとおりです。

* 分離された機能により、ローンチまでの時間を短縮できます。
* アップグレードが容易になりました。 カスタム機能はCommerce コードベースの外部にあり、アップグレード時の互換性の問題を防ぎます。
* 機能とロジックをCommerce外に移動することで、通常はプロセス内開発手法で使用されるリソースが解放されます。

## アーキテクチャ {#architecture}

標準のソリューションではなく、Adobe Developer App Builderは、Adobe CommerceなどのAdobeクラウドソリューションを拡張するための、一貫性のある標準化された共通の開発プラットフォームを提供します。例えば、次のようなものがあります。

* カスタムマイクロサービスおよび拡張機能の開発に使用するAdobe Developer Console。 プラグインや統合の作成に必要なすべてのツールや API にアクセスしながら、プロジェクトを構築および管理します。
* カスタム拡張機能および統合を構築するためのオープンソースツール、SDK およびライブラリ。 React Spectrum （Adobeの UI ツールキット）を使用すると、すべてのAdobeアプリに共通の UI を 1 つ用意できます。
* Adobeのサーバーレスプラットフォームでインフラストラクチャをホスティングするための I/O Runtime や、イベントベースの統合のための I/O Events などのサービスです。 また、Adobeでは、データやファイルの保存も標準でサポートされています。
* Adobe Experience Cloud:Experience Cloud組織で公開する拡張機能および統合機能を送信します。システム管理者は、これらの拡張機能を確認、管理、承認できます。 公開すると、カスタムのApp Builder拡張機能およびツールを他のAdobe Experience Cloud アプリと一緒に使用できるようになります。

次の図は、App Builder上に構築された標準アプリケーションでこれらの機能がどのように使用されるかを示しています。

![ アーキテクチャ ](/help/assets/app-builder/app-builder-architecture.jpeg)

App Builderのアーキテクチャについて詳しくは、[ アーキテクチャの概要 ](https://developer.adobe.com/app-builder/docs/guides/){target="_blank"} を参照してください。

## Amazon Sales Channel拡張機能 {#amazon-sales-channel-extension}

>[!IMPORTANT]
>
>Amazon Sales Channel拡張機能は現在開発中で、まだ正式にリリースされていません。  これらのビデオとチュートリアルは、Adobe Developer App Builderを実際のユースケースに使用する方法を示すことを目的としています。

次のチュートリアルでは、App Builder拡張機能を使用してAdobe CommerceをAmazon Sales Channelに接続する方法を示します。

* [技術概要App Builder](../app-builder/app-builder-technical-overview.md)
* [拡張フレームワーク](../app-builder/extensibility-framework-commerce-eventing.md)
* [機能デモApp Builder](../app-builder/app-builder-functional-demonstration.md)

## App Builderの基本を学ぶ {#additional-resources}

初期セットアップを含む、構成可能なコマース戦略の概要については、次のブログ投稿を参照してください。

[App Builderがコマースプラットフォームのビジネスの俊敏性をどのように促進するか ](https://business.adobe.com/blog/how-to/how-app-builder-helps-you-implement-a-composable-commerce-strategy){target="_blank"}

App Builderを使い始めるために、Adobeは次のドキュメントを作成しました。

* [App Builderの概要 ](https://developer.adobe.com/app-builder/docs/getting_started/){target="_blank"}

## ドキュメントを使用して学習を継続 {#appbuilder-documentation}

App Builderには、開発者向けのビデオとドキュメントが用意されています。ガイドや、独自のカスタムアプリケーションの開発に役立つリファレンスドキュメントなどです。

* [App Builder ドキュメント ](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [App Builder ビデオ ](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o){target="_blank"}

## サンプルアプリケーションを試す {#appbuilder-codesamples}

開発を開始する準備はできていますか？ 次のリンクには、作業を開始する際に役立つサンプルアプリケーションが含まれています。

* [Adobe Developer Web サイトのApp Builder コードラボ ](https://developer.adobe.com/app-builder/docs/resources/){target="_blank"}

## サポート {#support}

開発者向けサポートのリクエストについては、[Experience Leagueフォーラム ](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly){target="_blank"} を参照してください。

{{$include /help/_includes/app-builder-related-links.md}}

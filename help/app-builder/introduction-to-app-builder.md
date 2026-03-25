---
title: Adobe Commerceのすぐに利用できる拡張機能
description: Adobe App Builder と、それがプロセス外の拡張性の重要な側面になっている理由について説明します。
landing-page-description: App Builder の概要および Adobe Commerce の開発戦略に App Builder がどのように役立つかを説明します。
short-description: App Builder の概要および Adobe Commerce の開発戦略に App Builder がどのように役立つかを説明します。
kt: 11433
doc-type: tutorial
duration: 322
audience: all
last-substantial-update: 2023-2-16
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 94f8d82a-4a95-46ea-8eed-edf9bed5760c
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '728'
ht-degree: 6%

---

# App Builderの概要

従来、Adobe Commerceの開発では、インプロセスの拡張性を採用していました。 インプロセスモデルでは、アップグレード、サーバーのPHP バージョン、Commerceが使用するその他の多くの重要なサーバーアプリケーションやサービスと互換性を持つように、新しいコードを作成する必要があります。 Adobe Developer App Builderでは、これらの互換性の問題を回避するために、プロセス外の拡張機能を使用します。

## App Builder for Adobe Commerce {#app-builder}

>[!VIDEO](https://video.tv.adobe.com/v/3412839?learn=on)

Adobe Developer App Builderは、Adobeのソリューションを拡張するためのカスタムエクスペリエンスを統合および構築するためのサーバーレス拡張性プラットフォームです。Adobe Commerceでも利用できます。 App Builderなら、Commerceネイティブの機能を拡張し、サードパーティソリューションと統合して、安全性と拡張性の高いアプリを構築できます。 開発者は、Adobe Adobe Commerceを活用して、すぐにプロセスを終了できる拡張性を活用できます。これにより、短期的および長期的なメリットがもたらされます。

App Builderは、[!DNL Adobe Commerce]を拡張するカスタムアプリケーションを統合および作成するための統一されたサードパーティ拡張性フレームワークを提供します。 この拡張可能なフレームワークはAdobeのインフラストラクチャ上に構築されているため、開発者はカスタム マイクロサービスを構築し、他のAdobe ソリューションやサードパーティの統合を介して[!DNL Adobe Commerce]を拡張および統合できます。

App Builderは、様々な使用例で[!DNL Adobe Commerce]を拡張する方法を提供します。

* ミドルウェアの拡張性：カスタムコネクタを構築して、外部システムとAdobeアプリケーションを連携できます。また、事前に構築された一連の統合機能を利用できます。
* コアサービスの拡張性 – カスタム機能とビジネスロジックを使用してデフォルトの動作を拡張することで、コアアプリケーションの機能を拡張できます。
* ユーザーエクスペリエンスの拡張性 – コアエクスペリエンスを拡張してビジネス要件をサポートしたり、顧客固有のデジタルプロパティ、ストアフロント、バックオフィスアプリケーションを構築したりできます。

Adobe Developer App Builderはクラウドベースのソリューションなので、自動スケーリングされます。 このサービスは、地理的な場所に関係なく最高のパフォーマンスを実現できるように、グローバルに分散されています。

## App Builderについて詳しく知るべき理由

Adobe Commerceは完全なSAAS製品ではないため、開発するコードが複雑になり、アップグレードの問題が生じる可能性があります。 App Builderのような非プロセスの拡張性を使用することで、Adobe Commerce ストアに対して、インプロセスの手法を必要とせずに、独自のカスタム機能を提供できます。

その他の利点には、次のようなものがあります。

* 分離された機能により、ローンチまでの時間を短縮。
* アップグレードが容易になりました。 カスタム機能はCommerce コードベースの外部にあるため、アップグレード時の互換性の問題を防ぐことができます。
* Commerceの外部に機能やロジックを移行することで、インプロセス開発手法で通常使用されるリソースを解放します。

## デザイン {#architecture}

Adobe Developer App Builderでは、すぐに利用できるソリューションではなく、次のようなAdobeソリューションを拡張するための、一貫性のある標準化された共通の開発プラットフォームを提供します。

* カスタムマイクロサービスと拡張機能の開発に使用されるAdobe Developer Console。 プラグインや統合機能の作成に必要なあらゆるツールやAPIにアクセスしながら、プロジェクトを構築および管理できます。
* 独自の拡張機能や統合を構築できるオープンソースのツール、SDK、ライブラリ。 React Spectrum （AdobeのUI ツールキット）を使用して、すべてのAdobe アプリケーションに共通のUIを1つ用意します。
* Adobeのサーバーレスプラットフォーム上でインフラストラクチャをホスティングするためのI/O Runtimeや、イベントベースの統合用のI/O Eventsなどのサービス。 Adobeは、データやファイルを保存するためのすぐに使用できるサポートも提供しています。
* Experience Cloud組織で公開する拡張機能や統合機能を送信するAdobe Experience Cloud。システム管理者は、これらの拡張機能をレビュー、管理、承認できます。 公開すると、カスタムのApp Builder拡張機能とツールは、他のAdobe Experience Cloud アプリケーションと一緒に使用できるようになります。

次の図は、App Builder上に構築された標準アプリケーションがこれらの機能をどのように使用するかを示しています。

![ アーキテクチャ ](/help/assets/app-builder/app-builder-architecture.jpeg)

App Builder アーキテクチャについて詳しくは、[ アーキテクチャの概要](https://developer.adobe.com/app-builder/docs/guides/){target="_blank"}を参照してください。

## App Builder入門 {#additional-resources}

コンポーザブルコマース戦略の概要と、初期セットアップ方法については、次のブログ記事をご覧ください。

[App Builderがコマースプラットフォームのビジネスの俊敏性を高める方法](https://business.adobe.com/blog/how-to/how-app-builder-helps-you-implement-a-composable-commerce-strategy){target="_blank"}

App Builderの使用を開始するために、Adobeでは次のドキュメントを作成しました。

* [App Builder入門](https://developer.adobe.com/app-builder/docs/getting_started/){target="_blank"}

## ドキュメントで学習を続ける {#appbuilder-documentation}

App Builderでは、独自のカスタムアプリケーションの開発に役立つガイドやリファレンスドキュメントなど、開発者向けのビデオやドキュメントを提供しています。

* [App Builder ドキュメント ](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [App Builder ビデオ ](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o){target="_blank"}

## サンプルアプリケーションの1つを試す {#appbuilder-codesamples}

Adobe Marketo Engageをすぐ活用するには？ 次のリンクには、開始に役立つサンプルアプリケーションが含まれています。

* [Adobe Developer Web サイト上のApp Builder Code Labs](https://developer.adobe.com/app-builder/docs/resources/){target="_blank"}

## サポート {#support}

開発者サポートのリクエストについては、[Experience League フォーラム ](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly){target="_blank"}を利用してサポートを受けてください。

{{$include /help/_includes/app-builder-related-links.md}}

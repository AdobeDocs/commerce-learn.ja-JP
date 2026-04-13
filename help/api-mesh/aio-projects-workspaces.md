---
title: プロジェクトとワークスペースの操作
description: Adobe Developer Consoleを使用してプロジェクトとワークスペースを操作する方法を説明します。
jira: KT-11803
doc-type: Tutorial
duration: 593
last-substantial-update: 2023-02-08T00:00:00Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner, Intermediate
exl-id: ab51f68c-5d28-495b-8472-27b60c4aa8c1
source-git-commit: 003d55eac7e13a02ee633bed5ea9ab98825db151
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 0%

---

# プロジェクトとワークスペースの操作

このチュートリアルでは、メッシュを含めるプロジェクトとワークスペースの作成について説明します。 この作業は、主に[Adobe Developer コンソール &#x200B;](https://developer.adobe.com/console){target="_blank"} UIで行われます。

## この動画は誰のためのものでしょうか？

* Adobe Developer Console アカウントにアクセスでき、プロジェクトとワークスペースを作成する開発者。

## ビデオコンテンツ

* Adobe Developer Consoleでのプロジェクトとワークスペースの操作
* Adobe Developer ConsoleのワークスペースへのAPI メッシュの追加
* CLIでのAdobe Developer コンソールへのログイン
* 選択したプロジェクトとワークスペースをCLIで表示する
* 選択した組織、プロジェクト、またはワークスペースをCLIで変更する
* シンプルなAPI Mesh コマンドのテスト

>[!VIDEO](https://video.tv.adobe.com/v/3419740?captions=jpn&learn=on)

## Adobe Adobe Developer Consoleについて詳しく見る

Adobe Developer Consoleでは、APIが組織にどのように適合するかを次の階層で表します：`Organization > Project > Workspace > [API]`。 Adobe App Builderの詳細、コンソールへのログイン、基本的なトラブルシューティングについては、[最初のApp Builder アプリケーションの作成](https://developer.adobe.com/app-builder/docs/get_started/app_builder_get_started/first-app){target="_blank"}を参照してください。

## Adobe Developer Consoleでのプロジェクトの概要

Adobe Developer Consoleのすべての開発作業は、プロジェクトの一部として行われます。 プロジェクトには、1つまたは複数の製品、およびAPI、イベント、ランタイム、プラグインの組み合わせを含めることができます。 Adobe Developer コンソールのプロジェクトについて詳しくは、[&#x200B; プロジェクト &#x200B;](https://developer.adobe.com/developer-console/docs/guides/projects/){target="_blank"}を参照してください。

API Meshのコンテキストでプロジェクトとワークスペースを使用する方法について詳しくは、[&#x200B; プロジェクトとワークスペースの変更](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#modify-projects-and-workspaces){target="_blank"}を参照してください。

{{$include /help/_includes/api-mesh-related-links.md}}

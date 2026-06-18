---
title: Adobe I/O Runtime CLIおよびAPI Mesh プラグインのインストール
description: Adobe I/O Runtime コマンドラインインターフェイスとAPI Mesh プラグインをインストールして、Adobe Developer App Builder向けAPI Meshの使用を開始する方法を説明します。
jira: KT-11801
doc-type: Tutorial
duration: 410
last-substantial-update: 2023-02-08T00:00:00Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
source-git-commit: c73744d503de5023e5c001d0534200522db55b04
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 0%

---

# Adobe I/O Runtime CLIとMesh プラグインのインストール

Adobe Developer App Builder用API Meshの使用を開始する前に、`aio` CLIとAPI Mesh プラグインをインストールする必要があります。
インストール手順と前提条件については、API Mesh [はじめに](https://developer.adobe.com/graphql-mesh-gateway/mesh/basic/){target="_blank"} ページを参照してください。

## この動画は誰のためのものでしょうか？

* [Adobe I/O Runtime](https://developer.adobe.com/app-builder/docs/intro_and_overview/what-is-app-builder){target="_blank"}およびAPI Meshを使用した経験が限られているAPI Meshまたは[!DNL Adobe Commerce]を初めて利用する開発者。

## ビデオコンテンツ

* API メッシュの概要
* Adobe I/O Runtime CLIのインストール（コマンドラインインターフェイス）
* API Mesh プラグインのインストール

>[!VIDEO](https://video.tv.adobe.com/v/3419795?captions=jpn&learn=on)

## `aio` CLIおよびAPI Mesh プラグインのインストール

`aio` CLIをインストールするには、`node`と`npm`をインストールした後に次のコマンドを実行します。

```bash
npm install -g @adobe/aio-cli
```

Adobe I/O Runtime CLIをインストールしたら、次のコマンドを使用してAPI Mesh プラグインをインストールします。

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}

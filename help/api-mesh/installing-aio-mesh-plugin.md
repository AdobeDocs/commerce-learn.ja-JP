---
title: Adobe I/O Runtime コマンドラインインターフェイスとAPI Mesh プラグインのインストール
description: Adobe I/O Runtime コマンドラインインターフェイスとAPI Mesh プラグインのインストール方法について説明します
landing-page-description: Adobe App Builderを使用し、API Mesh プラグインを使用してAdobe I/O Runtimeをインストールする方法について説明します。
short-description: Adobe App Builderを使用し、API Mesh プラグインを使用してAdobe I/O Runtimeをインストールする方法について説明します。
kt: 11801
doc-type: tutorial
duration: 433
audience: all
last-substantial-update: 2023-2-8
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 0%

---

# Adobe I/O Runtime CLIとMesh プラグインのインストール

Adobe Developer App Builder用API Meshの使用を開始する前に、`aio` CLIとAPI Mesh プラグインをインストールする必要があります。
インストール手順と前提条件については、API Mesh [はじめに](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/){target="_blank"} ページを参照してください。

## この動画は誰のためのものでしょうか？

* [!DNL Adobe Commerce]Adobe I/O Runtime[およびAPI Meshを使用した経験が限られているAPI Meshまたは](https://developer.adobe.com/runtime/docs/guides/overview/){target="_blank"}を初めて利用する開発者。

## ビデオコンテンツ

* API メッシュの概要
* Adobe I/O Runtime CLIのインストール（コマンドラインインターフェイス）
* API Mesh プラグインのインストール

>[!VIDEO](https://video.tv.adobe.com/v/3419795?captions=jpn&learn=on)

## `aio` CLIおよびAPI Mesh プラグインのインストール

`node`と`npm`をインストールした後、次のコマンドを実行して`aio` CLIをインストールします。

```bash
npm install -g @adobe/aio-cli
```

Adobe I/O Runtime CLIをインストールしたら、次のコマンドを使用してAPI Mesh プラグインをインストールします。

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}

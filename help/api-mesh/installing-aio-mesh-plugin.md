---
title: Adobe I/O Runtime CLIおよびAPI Mesh プラグインのインストール
description: Adobe I/O Runtime コマンドラインインターフェイスとAPI Mesh プラグインをインストールして、Adobe Developer App Builder向けAPI Meshの使用を開始する方法を説明します。
jira: KT-11801
doc-type: Tutorial
duration: 410
last-substantial-update: 2023-02-08T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 72863f3c-9d27-5dda-afe1-d9f934b1fba0
    internal-label: Extensibility
  - id: b48dbafb-4193-5648-b9d9-bf96e9c9a411
    internal-label: Backend Development
  - id: c4f010fa-1478-4300-a88d-706fbc036a7a
    internal-label: APIs and SDKs
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
  - id: 125c1f49-aefd-5f34-a252-288937f95f6b
    internal-label: Marketing Tools
subfeature_v2:
  - id: ce84ce08-883f-4337-ae83-6bb1855ca732
    internal-label: API Mesh
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
  - id: 618ab558-d6ad-5352-99d6-d5702c6fdf80
    internal-label: Tools and External Services
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: 7516911fc14bc80f327310b99a00f9cbff7c65c7
workflow-type: tm+mt
source-wordcount: '182'
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

>[!VIDEO](https://video.tv.adobe.com/v/3414122?learn=on)

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

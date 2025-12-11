---
title: Adobe I/O Runtime コマンドラインインターフェイスと API メッシュプラグインのインストール
description: Adobe I/O Runtime コマンドラインインターフェイスと API メッシュプラグインのインストール方法の確認
landing-page-description: Adobe App Builderの使用方法と、Adobe I/O Runtime with API Mesh プラグインのインストール方法を説明します。
short-description: Adobe App Builderの使用方法と、Adobe I/O Runtime with API Mesh プラグインのインストール方法を説明します。
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 0%

---

# Adobe I/O Runtime CLI とメッシュプラグインのインストール

Adobe Developer App Builderの API メッシュの使用を開始する前に、`aio` CLI と API メッシュプラグインをインストールする必要があります。
インストール手順と前提条件については、API メッシュ [&#x200B; はじめに &#x200B;](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/){target="_blank"} ページを参照してください。

## このビデオの目的は誰ですか。

* API メッシュまたは [!DNL Adobe Commerce] を初めて使用する開発者で、[Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/){target="_blank"} と API メッシュを使用する経験が限られているユーザー。

## ビデオコンテンツ

* API メッシュの概要
* Adobe I/O Runtime CLI （コマンドラインインターフェイス）のインストール
* API メッシュプラグインのインストール

>[!VIDEO](https://video.tv.adobe.com/v/3419795?captions=jpn&quality=12&learn=on)

## `aio` CLI および API メッシュプラグインのインストール

`node` および `npm` のインストール後、次のコマンドを実行して `aio` CLI をインストールします。

```bash
npm install -g @adobe/aio-cli
```

Adobe I/O Runtime CLI がインストールされたら、次のコマンドを使用して API メッシュプラグインをインストールします。

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}

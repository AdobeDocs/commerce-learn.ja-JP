---
title: Adobe I/O Runtimeコマンドラインインターフェイスと API Mesh プラグインのインストール
description: Adobe I/O Runtimeコマンドラインインターフェイスと API Mesh プラグインのインストール方法を説明します
landing-page-description: App Builder の使用方法と、API MeshAdobeを使用したAdobe I/O Runtimeのインストール方法を説明します。
short-description: Discover how to use Adobe App Builder and install the Adobe I/O Runtime with API Mesh plugin.
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: d85426bcf3ae0412a433414d70c874964905dda0
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Adobe I/O Runtime CLI および Mesh プラグインのインストール

Adobe Developer App Builder で API Mesh を使用する前に、 `aio` CLI と API Mesh プラグイン
インストールの手順と前提条件については、 API Mesh を参照してください。 [はじめに](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/){target="_blank"} ページ。

## このビデオは誰のためのものですか？

* API Mesh を初めて使用する開発者または [!DNL Adobe Commerce] ～を使った経験が限られている [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/){target="_blank"} と API メッシュ

## ビデオコンテンツ

* API メッシュの概要
* Adobe I/O Runtime CLI（コマンドラインインターフェイス）のインストール
* API Mesh プラグインのインストール

>[!VIDEO](https://video.tv.adobe.com/v/3414122?quality=12&learn=on)

## のインストール `aio` CLI および API Mesh プラグイン

インストール後 `node` および `npm`、次のコマンドを実行して、 `aio` CLI:

```bash
npm install -g @adobe/aio-cli
```

Adobe I/O Runtime CLI をインストールしたら、次のコマンドを使用して API Mesh プラグインをインストールします。

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}

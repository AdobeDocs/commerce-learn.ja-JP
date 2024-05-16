---
title: Adobe I/O Runtime コマンドラインインターフェイスと API メッシュプラグインのインストール
description: Adobe I/O Runtime コマンドラインインターフェイスと API メッシュプラグインのインストール方法の確認
landing-page-description: Adobe App Builder を使用し、API メッシュ プラグインを使用してAdobe I/O Runtimeをインストールする方法を確認します。
short-description: Adobe App Builder を使用し、API メッシュ プラグインを使用してAdobe I/O Runtimeをインストールする方法を確認します。
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 0%

---

# Adobe I/O Runtime CLI とメッシュプラグインのインストール

Adobe Developer App Builder の API メッシュの使用を開始する前に、 `aio` CLI と API メッシュ プラグイン。
インストール手順と前提条件については、API メッシュを参照してください。 [はじめに](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/){target="_blank"} ページ。

## このビデオの目的は誰ですか。

* API メッシュを初めて使用する開発者、または [!DNL Adobe Commerce] の使用経験が限られているユーザー [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/){target="_blank"} と API メッシュです。

## ビデオコンテンツ

* API メッシュの概要
* Adobe I/O Runtime CLI （コマンドラインインターフェイス）のインストール
* API メッシュプラグインのインストール

>[!VIDEO](https://video.tv.adobe.com/v/3414122?quality=12&learn=on)

## のインストール `aio` CLI および API メッシュ プラグイン

インストール後 `node` および `npm`。次のコマンドを実行して、 `aio` CLI:

```bash
npm install -g @adobe/aio-cli
```

Adobe I/O Runtime CLI がインストールされたら、次のコマンドを使用して API メッシュプラグインをインストールします。

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}

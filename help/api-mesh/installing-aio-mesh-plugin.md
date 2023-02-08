---
title: Adobe Developer IO コマンドラインインターフェイスと API Mesh プラグインのインストール
description: Adobe Developer IO コマンドラインインターフェイスと API Mesh プラグインのインストール方法を説明します
landing-page-description: App Builder の使用方法と、API MeshAdobeを使用したAdobe Developer IO のインストール方法を説明します。
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Adobe Developer IO および Mesh プラグインのインストール

開始する前に、いくつかの設定が必要です。 まず、Adobe Developer IO コマンドラインインターフェイスの設定を行います。 次に、各環境で API Mesh プラグインが設定されていることを確認します。
Node、nvm を実行するためのローカル環境の設定とAdobe Developer I/O のインストールの手順については、必ず [GraphQL Mesh はじめに](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/).

## このビデオは誰のためのものですか？

* App Builder を初めて使用するAdobe、または [!DNL Magento Open Source] Adobe Developer IO と API Mesh の使用経験が限られています。

## ビデオコンテンツ

* API メッシュの概要
* Adobe Developer IO コマンドラインインターフェイスのインストール
* AIO コマンドラインへの API Mesh プラグインの追加

>[!VIDEO](https://video.tv.adobe.com/v/3414122/)

## NPM および AIO を使用したコマンドの例

Adobe Developerコマンドラインインターフェイスのインストールは簡単です。 Node をインストールした後、次のコマンドを実行します。 `npm install -g @adobe/aio-cli`
Adobe Developer cli をインストールすると、mesh プラグインをインストールできます。 これを行うには、次のコマンドを実行します。 `aio plugins:install @adobe/aio-cli-plugin-api-mesh`

{{$include /help/_includes/api-mesh-related-links.md}}

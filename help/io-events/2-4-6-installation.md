---
title: Adobe Commerce 2.4.6 の IO イベントをインストールする方法を説明します
description: Adobe Developer App Builderで使用するために、Adobe Commerce 2.4.6 で IO イベントに必要なモジュールをインストールする方法を説明します
landing-page-description: Adobe Commerce 2.4.6 で必要な複数のモジュールをインストールする方法を説明します。
short-description: Adobe Commerce 2.4.6 で必要な複数のモジュールをインストールする方法を説明します。
kt: 11887
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: Adobe Commerce 2.4.6
feature: App Builder, Eventing
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 41b31ed8-04c5-4d50-aaff-abc3718b5957
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 0%

---

# Adobe Commerce 2.4.6 のインストール

Composer for version 2.4.6 を使用してAdobe Commerceに複数の新しいモジュールをインストールする方法を説明します。追加ドキュメントについては、[Adobe Commerce用のAdobe I/O Eventsのインストール ](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"} を参照してください。

## このビデオの目的は誰ですか。

* I/O イベントを使用してAdobe CommerceとAdobe Developer App Builderを初めて使用する開発者。

## ビデオコンテンツ {#video-content}

* オンプレミスホスティング用に実行するコマンド
* Adobe Commerce Cloud 用に実行するコマンド
* Adobe Commerce Cloud yaml の必須編集

>[!VIDEO](https://video.tv.adobe.com/v/3415795?quality=12&learn=on)

## 便利なコマンド {#useful-commands}

セルフホスト環境で使用しているか、Adobe Commerce Cloud を使用しているかに応じて、わずかに異なる様々なコマンドがあります。

### オンプレミスホスティング {#on-premise}

```bash
bin/magento events:generate:module

bin/magento module:enable --all

bin/magento setup:upgrade && bin/magento setup:di:compile
```

### クラウド上のAdobe Commerce {#adobe-commerce-cloud}

```bash
composer info magento/ece-tools
```

Commerce Cloud`.magento.env.yaml`:

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}

---
title: Adobe Commerce 2.4.6 の IO イベントのインストール方法を説明します。
description: Adobe Commerce 2.4.6 で I/O イベントに必要なモジュールをAdobe Developer App Builder で使用するためにインストールする方法を説明します
landing-page-description: Adobe Commerce 2.4.6 に必要なモジュールをいくつかインストールする方法を説明します。
kt: 11887
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: "Adobe Commerce 2.4.6"
source-git-commit: 4f73e2a6852d545d9b30b03158b6d8a35e3eba49
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Adobe Commerce 2.4.6 のインストール

バージョン 2.4.6 用の Composer を使用して、Adobe Commerceに複数の新しいモジュールをインストールする方法を説明します。追加ドキュメントについては、を参照してください。 [Adobe CommerceのAdobe I/Oイベントのインストール](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## このビデオは誰のためのものですか？

* I/O イベントを使用した、Adobe CommerceとAdobe Developer App Builder を初めて利用する開発者。

## ビデオコンテンツ {#video-content}

* オンプレミスホスティング用に実行するコマンド
* Adobe Commerce Cloud用に実行するコマンド
* Adobe Commerce Cloud yaml が必要な編集

>[!VIDEO](https://video.tv.adobe.com/v/3415795)

## 便利なコマンド {#useful-commands}

自己ホスト型の環境を使用しているか、Adobe Commerce Cloudを使用しているかに応じて、多少異なる様々なコマンドがあります。

### オンプレミスホスティング {#on-premise}

```bash
bin/magento events:generate:module

bin/magento module:enable --all

bin/magento setup:upgrade && bin/magento setup:di:compile
```

### Adobe Commerce on Cloud {#adobe-commerce-cloud}

```bash
composer info magento/ece-tools
```

Commerce Cloud `.magento.env.yaml`:

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}
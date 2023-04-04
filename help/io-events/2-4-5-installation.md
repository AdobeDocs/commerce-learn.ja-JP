---
title: Adobe Commerce 2.4.5 の IO イベントのインストール方法を説明します。
description: Adobe Commerce 2.4.5 で I/O イベントに必要なモジュールをAdobe Developer App Builder で使用するためにインストールする方法を説明します
landing-page-description: Composer を使用してAdobe Commerce 2.4.5 に必要なモジュールをいくつかインストールする方法を説明します。
short-description: Learn how to install several modules needed for Adobe Commerce 2.4.5 using composer.
kt: 11886
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: "Adobe Commerce 2.4.5"
source-git-commit: d85426bcf3ae0412a433414d70c874964905dda0
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Adobe Commerce 2.4.5 のインストール

バージョン 2.4.5 の Composer を使用して、Adobe Commerceに複数の新しいモジュールをインストールする方法を説明します。これにより、Adobe Commerceアプリケーションで使用する必要なモジュールが設定されます。 その他のドキュメントは、 [Adobe CommerceのAdobe I/Oイベントのインストール](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## このビデオは誰のためのものですか？

* I/O イベントを使用した、Adobe CommerceとAdobe Developer App Builder を初めて利用する開発者

## ビデオコンテンツ {#video-content}

* Composer を使用した必要なモジュールのインストール
* オンプレミスホスティング用に実行するコマンド
* Adobe Commerce Cloud用に実行するコマンド
* Adobe Commerce Cloud yaml が必要な編集

>[!VIDEO](https://video.tv.adobe.com/v/3415794?quality=12&learn=on)

## 便利なコマンド {#useful-commands}

自己ホスト型の環境を使用しているか、Adobe Commerce Cloudを使用しているかに応じて、多少異なる様々なコマンドがあります。

### オンプレミスホスティング {#on-premise}

```bash
composer require magento/commerce-eventing=^1.0 --no-update

composer update

bin/magento events:generate:module

bin/magento module:enable --all

bin/magento setup:upgrade && bin/magento setup:di:compile
```

### Adobe Commerce on Cloud {#adobe-commerce-cloud}

```bash
composer require magento/commerce-eventing=^1.0 --no-update

composer update

composer info magento/ece-tools
```

Commerce Cloud `.magento.env.yaml`:

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}

---
title: Adobe Commerce 2.4.5のIO イベントのインストール方法について説明します
description: Adobe Developer App Builderで使用するAdobe Commerce 2.4.5のIO イベントに必要なモジュールをインストールする方法について説明します
landing-page-description: Composerを使用して、Adobe Commerce 2.4.5に必要な複数のモジュールをインストールする方法について説明します。
short-description: Composerを使用して、Adobe Commerce 2.4.5に必要な複数のモジュールをインストールする方法について説明します。
kt: 11886
doc-type: tutorial
duration: 214
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: Adobe Commerce 2.4.5
feature: App Builder, Eventing
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: e0adfd85-5a3d-44ba-aab5-ecd7c61715cf
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 0%

---

# Adobe Commerce 2.4.5 インストール

バージョン 2.4.5のComposerを使用して、Adobe Commerceにいくつかの新しいモジュールをインストールする方法について説明します。これは、Adobe Commerce アプリケーションで使用する必要なモジュールを設定します。 その他のドキュメントについては、[Adobe Commerce用Adobe I/O Eventsのインストール &#x200B;](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}を参照してください。

## この動画は誰のためのものでしょうか？

* I/O Eventsを使用したAdobe CommerceとAdobe Developer App Builderの初心者の開発

## ビデオコンテンツ {#video-content}

* Composerを使用した必要なモジュールのインストール
* オンプレミスホスティング用に実行するコマンド
* Adobe Commerce Cloudのコマンド実行
* Adobe Commerce Cloud yaml必要な編集

>[!VIDEO](https://video.tv.adobe.com/v/3419829?captions=jpn&learn=on)

## 便利なコマンド {#useful-commands}

セルフホスト環境を使用している場合とAdobe Commerce Cloudを使用している場合で、若干異なる様々なコマンドがあります。

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

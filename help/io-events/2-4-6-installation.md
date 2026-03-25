---
title: Adobe Commerce 2.4.6のIO イベントのインストール方法について説明します
description: Adobe Developer App Builderで使用するAdobe Commerce 2.4.6のIO イベントに必要なモジュールをインストールする方法について説明します
landing-page-description: Adobe Commerce 2.4.6に必要な複数のモジュールをインストールする方法について説明します。
short-description: Adobe Commerce 2.4.6に必要な複数のモジュールをインストールする方法について説明します。
kt: 11887
doc-type: tutorial
duration: 167
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: Adobe Commerce 2.4.6
feature: App Builder, Eventing
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 41b31ed8-04c5-4d50-aaff-abc3718b5957
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 0%

---

# Adobe Commerce 2.4.6 インストール

バージョン 2.4.6のComposerを使用して、Adobe Commerceに新しいモジュールをインストールする方法について説明します。その他のドキュメントについては、[Adobe Commerce用Adobe I/O Eventsのインストール &#x200B;](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}を参照してください。

## この動画は誰のためのものでしょうか？

* I/O Eventsを使用してAdobe CommerceとAdobe Developer App Builderを初めて使用する開発者。

## ビデオコンテンツ {#video-content}

* オンプレミスホスティング用に実行するコマンド
* Adobe Commerce Cloudのコマンド実行
* Adobe Commerce Cloud yaml必要な編集

>[!VIDEO](https://video.tv.adobe.com/v/3415795?learn=on)

## 便利なコマンド {#useful-commands}

セルフホスト環境を使用している場合とAdobe Commerce Cloudを使用している場合で、若干異なる様々なコマンドがあります。

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

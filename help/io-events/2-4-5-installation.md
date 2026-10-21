---
title: Adobe Commerce 2.4.5のIO イベントのインストール方法について説明します
description: Adobe Developer App Builderで使用するAdobe Commerce 2.4.5のIO イベントに必要なモジュールをインストールする方法について説明します
jira: KT-11886
doc-type: Tutorial
duration: 179
last-substantial-update: 2023-02-22T00:00:00.000Z
badge: Adobe Commerce 2.4.5
feature: App Builder, Eventing
topic: Commerce, Architecture
role: Developer
level: Beginner, Intermediate
exl-id: e0adfd85-5a3d-44ba-aab5-ecd7c61715cf
TQID: 'https://experienceleague.adobe.com/vb-q-JXeM4KvkxzfDui1MaTOSBFXDVBwmpTeolUtKGw'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
  - id: 5594f40c-5dc7-522f-a0e0-f84045197b3c
    internal-label: Eventing
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
subfeature_v2:
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: 7516911fc14bc80f327310b99a00f9cbff7c65c7
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 0%
---
# Adobe Commerce 2.4.5 インストール

バージョン 2.4.5のComposerを使用して、Adobe Commerceにいくつかの新しいモジュールをインストールする方法について説明します。 これは、Adobe Commerce アプリケーションで使用する必要なモジュールを設定します。 その他のドキュメントについては、[Adobe Commerce用Adobe I/O Eventsのインストール &#x200B;](https://developer.adobe.com/commerce/extensibility/events/installation){target="_blank"}を参照してください。

## この動画は誰のためのものでしょうか？

* I/O Eventsを使用したAdobe CommerceとAdobe Developer App Builderの初心者の開発

## ビデオコンテンツ {#video-content}

* Composerを使用した必要なモジュールのインストール
* オンプレミスホスティング用に実行するコマンド
* Adobe Commerce Cloudのコマンド実行
* Adobe Commerce Cloud yaml必要な編集

>[!VIDEO](https://video.tv.adobe.com/v/3415794?learn=on)

## 使用可能なコマンド {#useful-commands}

セルフホスト環境を使用しているか、Adobe Commerce Cloudを使用しているかによって、若干異なる様々なコマンドがあります。

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



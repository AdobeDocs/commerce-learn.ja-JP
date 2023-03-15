---
title: Adobe Commerceの設定
description: Adobe Developer App Builder でイベントの使用を許可するようにAdobe Commerceを設定する方法について説明します。
landing-page-description: Adobe Developer App Builder での消費にイベントメカニズムを使用するようにAdobe Commerceを設定する方法を説明します。
short-description: Learn how to configure Adobe Commerce to use the event mechanism for consumption by Adobe Developer App Builder.
kt: 11889
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---


# Adobe Commerceの設定

イベントを公開するようにAdobe Commerceを設定する方法を説明します。 その他のドキュメントは、 [Adobe CommerceのAdobe I/Oイベントのインストール](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## このビデオは誰のためのものですか？

* I/O イベントを使用したAdobe CommerceおよびAdobe Developer App Builder を初めて使用する開発者は、アプリビルダープロジェクトをAdobeする必要があります。

## ビデオコンテンツ {#video-content}

* コマース管理でのAdobe I/Oイベントの設定
* コマース管理での秘密鍵の保存
* コマース管理での一意の識別子の保存
* イベントプロバイダーの作成

>[!VIDEO](https://video.tv.adobe.com/v/3415799)

## 便利なコマンド {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}

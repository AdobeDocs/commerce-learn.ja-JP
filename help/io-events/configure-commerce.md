---
title: Adobe Commerceの設定
description: Adobe Developer App Builder でイベントの使用を許可するようにAdobe Commerceを設定する方法について説明します。
landing-page-description: Adobe Developer App Builder での消費にイベントメカニズムを使用するようにAdobe Commerceを設定する方法を説明します。
short-description: Adobe Developer App Builder での消費にイベントメカニズムを使用するようにAdobe Commerceを設定する方法を説明します。
kt: 11889
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Configuration, Backend Development
topic: Commerce, Architecture
role: Architect, Developer, User
level: Beginner, Intermediate
exl-id: b8062042-2e90-4750-92ef-d55a76f2d842
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '150'
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

>[!VIDEO](https://video.tv.adobe.com/v/3415799?quality=12&learn=on)

## 便利なコマンド {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}

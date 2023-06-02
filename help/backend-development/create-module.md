---
title: モジュールの作成
description: 1 つのパラメーターで json を返すページを作成します。
topic: Development
kt: 5614
doc-type: video
activity: use
last-substantial-update: 2023-6-2
exl-id: 941c04ee-54b8-4b81-b77d-fff5875927f0
source-git-commit: fb2cb5b844c4156802e8627e71fc18f8e7fa981d
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# モジュールの作成

モジュールは、 [!DNL Commerce]  — システム全体がモジュールに基づいて構築されます。 通常、カスタマイズを作成する最初の手順は、モジュールを構築することです。

## このビデオは誰のためのものですか？

- 開発者

## モジュールを追加する手順

- モジュールフォルダーを作成します。
- etc/module.xmlファイルを作成します。
- registration.php ファイルを作成します。
- bin/magento の設定を実行します。
- 新しいモジュールをインストールするには、スクリプトをアップグレードします。
- モジュールが動作していることを確認します。

>[!VIDEO](https://video.tv.adobe.com/v/35792?learn=on)

### module.xml

```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="Training_Sales">
        <sequence>
            <module name="Magento_Sales"/>
        </sequence>
    </module>
</config>
```

### registration.php

```PHP
<?php

use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,
    'Training_Sales',
    __DIR__);
```

## 役に立つリソース

- [モジュールリファレンスガイド](https://developer.adobe.com/commerce/php/module-reference/)

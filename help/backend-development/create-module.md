---
title: モジュールの作成
description: Adobe Commerceでモジュールを作成して登録し、設定を実行して、管理領域、ストアフロント、REST API コンテキストのPSR ロガーにログを記録するプラグインを追加します。
jira: KT-5614
doc-type: Technical Video
duration: 1113
activity: use
last-substantial-update: 2026-03-23T00:00:00Z
feature: Configuration, System, Backend Development
topic: Commerce, Development
role: Admin, Developer
level: Beginner, Intermediate
exl-id: 941c04ee-54b8-4b81-b77d-fff5875927f0
source-git-commit: 1e67193c9b80c929ec391acef771562fb930cc67
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 0%

---

# モジュールの作成

モジュールは[!DNL Commerce]の構造要素です。モジュールはシステムのバックボーンを形成します。 通常、モジュールを作成してカスタマイズを開始します。

## この動画は誰のためのものでしょうか？

* バックエンド開発者

## モジュールを追加する手順

1. モジュールフォルダーを作成します。
2. `etc/module.xml` ファイルを作成します。
3. `registration.php` ファイルを作成します。
4. `bin/magento setup:upgrade`を実行して、モジュールを登録およびインストールします。
5. モジュールが機能していることを確認します。

>[!VIDEO](https://video.tv.adobe.com/v/3412454?captions=jpn&learn=on)

### module.xml ファイル

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

### registration.php ファイル

```php
<?php

use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,
    'Training_Sales',
    __DIR__);
```

### プラグインを追加

次に、基本モジュールに機能を追加します。 Adobe Commerceの開発では、プラグインを必須ツールとして使用します。 このビデオとチュートリアルでは、プラグインの作成方法について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3420255?learn=on)

### プラグインで覚えておくべきこと

* `di.xml`ですべてのプラグインを宣言します。
* 各プラグインに一意の名前を付けます。
* オプションで`disabled`属性と`sortOrder`属性を設定できます。
* プラグインの範囲を設定するには、`di.xml` ファイルを含むフォルダーを選択します。
* ターゲット メソッド呼び出しの前、後、または周囲にプラグインを実行します。
* `around`個のプラグインは使用しないでください。 彼らはあなたを誘惑しますが、彼らはしばしば間違った選択を表し、パフォーマンスの問題を引き起こします。

### プラグインコードサンプル

このチュートリアルでは、次のXML クラスとPHP クラスを使用して、最初のモジュールにプラグインを追加します。

### app/code/Training/Sales/etc/adminhtml/di.xml

```xml
<?xml version="1.0" ?>
<!--
/**
 * Copyright &copy; Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <!-- A Plugin that executes when the admin user places an order -->
    <type name="Magento\Sales\Model\Order">
        <plugin name="admin-training-sales-add-logging" type="Training\Sales\Plugin\AdminAddLoggingAfterOrderPlacePlugin" disabled="false" sortOrder="0"/>
    </type>
</config>
```

### app/code/Training/Sales/etc/frontend/di.xml

```xml
<?xml version="1.0" ?>
<!--
/**
 * Copyright &copy; Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <!-- A plugin that executes when a customer uses the LoginPost controller from the Luma frontend -->
    <type name="Magento\Customer\Controller\Account\LoginPost">
        <plugin name="training-customer-loginpost-plugin"
                type="Training\Sales\Plugin\CustomerLoginPostPlugin" sortOrder="20"/>
    </type>
</config>
```

### app/code/Training/Sales/etc/webapi_rest/di.xml

```xml
<?xml version="1.0" ?>
<!--
/**
 * Copyright &copy; Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <!-- A plugin that executes when the REST API is used OR when the Luma frontend places an order -->
    <type name="Magento\Sales\Model\Order">
        <plugin name="rest-training-sales-add-logging" type="Training\Sales\Plugin\RestAddLoggingAfterOrderPlacePlugin"/>
    </type>
</config>
```

### app/code/Training/Sales/Plugin/AdminAddLoggingAfterOrderPlacePlugin.php

```php
<?php

declare(strict_types=1);

namespace Training\Sales\Plugin;

use Magento\Sales\Model\Order;
use Psr\Log\LoggerInterface;

/**
 *
 */
class AdminAddLoggingAfterOrderPlacePlugin
{

    /**
     * @var LoggerInterface
     */
    private LoggerInterface $logger;

    /**
     * @param LoggerInterface $logger
     */
    public function __construct(
        LoggerInterface $logger
    ) {
        $this->logger = $logger;
    }

    /**
     * Add log after an order is placed
     *
     * @param Order $subject
     * @param Order $result
     * @return Order
     */
    public function afterPlace(Order $subject, Order $result): Order
    {
        // Log this admin area order
        $this->logger->notice('An ADMIN User placed an Order - $' . $subject->getBaseTotalDue());
        return $result;
    }
}
```

### app/code/Training/Sales/Plugin/CustomerLoginPostPlugin.php

```php
<?php

declare(strict_types=1);

namespace Training\Sales\Plugin;

use Psr\Log\LoggerInterface;
use Magento\Framework\App\RequestInterface;

/**
 * Introduces Context information for ActionInterface of Customer Action
 */
class CustomerLoginPostPlugin
{

    /**
     * @var LoggerInterface
     */
    private LoggerInterface $logger;

    /**
     * @var RequestInterface
     */
    private RequestInterface $request;

    /**
     * @param LoggerInterface $logger
     * @param RequestInterface $request
     */
    public function __construct(LoggerInterface $logger, RequestInterface $request)
    {
        $this->logger = $logger;
        $this->request = $request;
    }

    /**
     * Simple example of a before Plugin on a public method in a Controller
     */
    public function beforeExecute()
    {
        $login = $this->request->getParam('login');
        $username = $login['username'];
        $this->logger->notice( "Login Post controller was used for " . $username );
    }
}
```

### app/code/Training/Sales/Plugin/RestAddLoggingAfterOrderPlacePlugin.php

```php
<?php

declare(strict_types=1);

namespace Training\Sales\Plugin;

use Magento\Sales\Model\Order;
use Psr\Log\LoggerInterface;

/**
 *
 */
class RestAddLoggingAfterOrderPlacePlugin
{

    /**
     * @var LoggerInterface
     */
    private LoggerInterface $logger;

    /**
     * @param LoggerInterface $logger
     */
    public function __construct(
        LoggerInterface $logger
    ) {
        $this->logger = $logger;
    }

    /**
     * Add log after an order is placed
     *
     * @param Order $subject
     * @param Order $result
     * @return Order
     */
    public function afterPlace(Order $subject, Order $result): Order
    {
        // Log this customer driven order
        $this->logger->notice('A Customer placed an Order for $' . $subject->getBaseTotalDue());
        return $result;
    }
}
```

## 役立つリソース

* [&#x200B; モジュール参照ガイド &#x200B;](https://developer.adobe.com/commerce/php/module-reference/){target="_blank"}
* [&#x200B; プラグイン &#x200B;](https://developer.adobe.com/commerce/php/development/components/plugins/){target="_blank"}

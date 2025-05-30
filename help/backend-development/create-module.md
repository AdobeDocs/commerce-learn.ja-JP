---
title: モジュールの作成
description: PSR ロガーに情報を送信するモジュールをAdobe Commerceで作成する方法を説明します。 これにより、Adobe Commerceの最初のモジュールに機能が追加されます。
kt: 5614
doc-type: video
activity: use
last-substantial-update: 2023-6-2
feature: Configuration, System, Backend Development
topic: Commerce, Development
role: Admin, Developer
level: Beginner, Intermediate
exl-id: 941c04ee-54b8-4b81-b77d-fff5875927f0
source-git-commit: 4f6c8abec90663f80233b94456ad1e58edb86d51
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 0%

---

# モジュールの作成

モジュールは [!DNL Commerce] の構成要素です。システム全体がモジュールに基づいて構築されます。 通常、カスタマイズを作成する最初の手順は、モジュールを構築することです。

## このビデオの目的は誰ですか。

- 開発者

## モジュールの追加手順

- モジュールフォルダーを作成します。
- etc/module.xml ファイルを作成します。
- registration.php ファイルを作成します。
- bin/magento 設定を実行します。
- スクリプトをアップグレードして、新しいモジュールをインストールします。
- モジュールが動作していることを確認します。

>[!VIDEO](https://video.tv.adobe.com/v/3412454?learn=on&captions=jpn)

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

```php
<?php

use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,
    'Training_Sales',
    __DIR__);
```

### プラグインの追加と機能の提供

次の手順では、基本モジュールにいくつかの機能を追加します。 プラグインは、すべてのAdobe Commerce開発者が使用する不可欠なツールです。 このビデオとチュートリアルは、プラグインの作成に役立ちます。

>[!VIDEO](https://video.tv.adobe.com/v/3420255?learn=on)

### プラグインの注意事項

- すべてのプラグインは `di.xml` で宣言されます。
- プラグイン名は一意である必要があります
- disabled と sortOrder はオプションです
- プラグインのスコープは、内部のフォルダーによって設定されます
- プラグインは、メソッドが呼び出される前、後、またはその両方で実行できます
- `around` プラグインの使用は避けてください。 これらは使用したくなりますが、多くの場合、誤った選択であり、パフォーマンスの問題を引き起こします。

### プラグインのコードサンプル

最初のモジュールにプラグインを追加する際に、チュートリアルで使用する XML クラスと PHP クラスを以下に示します

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

## 役に立つリソース

- [ モジュールリファレンスガイド ](https://developer.adobe.com/commerce/php/module-reference/){target="_blank"}
- [ プラグイン ](https://developer.adobe.com/commerce/php/development/components/plugins/){target="_blank"}

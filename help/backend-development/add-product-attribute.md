---
title: 製品属性の作成
description: 1つのパラメーターでjsonを返すページを作成します。
kt: 14131
doc-type: video
duration: 605
activity: use
last-substantial-update: 2023-2-10
feature: Configuration, System, Backend Development
topic: Commerce, Development
role: Admin, User
level: Beginner, Intermediate
exl-id: 98257e62-b23d-4fa9-a0eb-42e045c53195
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '305'
ht-degree: 0%

---

# 製品属性の作成

製品属性の追加は、[!DNL Commerce]で最も人気のある操作の1つです。 属性は、製品に関連する多くの実用的なタスクを解決する強力な方法です。 ドロップダウンタイプ属性を製品に追加する簡単なプロセスがあります。

このビデオの内容：

* Cotton、Leather、Silk、Denim、Fur、Woolの値を持つclothing_materialという属性を追加します
* この属性を製品ビューページに太字で表示します
* デフォルトの属性セットに割り当て、制限を追加します
* 新しい属性を追加

## この動画は誰のためのものでしょうか？

* コマースを初めて利用する開発者で、プログラムから商品属性を作成する方法を学ぶ必要がある場合

## ビデオコンテンツ

>[!VIDEO](https://video.tv.adobe.com/v/3412441?captions=jpn&learn=on)

## コードサンプル

最初に、必要なフォルダー、xmlおよびPHP ファイルを作成します。

* app/code/Learning/ClothingMaterial/registration.php
* app/code/Learning/ClothingMaterial/etc/module.xml
* app/code/Learning/ClothingMaterial/Model/Attribute/Backend/Material.php
* app/code/Learning/ClothingMaterial/Model/Attribute/Frontend/Material.php
* app/code/Learning/ClothingMaterial/Model/Attribute/Source/Material.php
* app/code/Learning/ClothingMaterial/Setup/InstallData.php

### app/code/Learning/ClothingMaterial/registration.php

```php
<?php

use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,
    'Learning_ClothingMaterial',
    __DIR__);
```

### app/code/Learning/ClothingMaterial/etc/module.xml

>[!NOTE]
>
>モジュールで宣言スキーマを使用しており、ほとんどのモジュールで2.3.0以降を使用している場合は、setup_versionを省略する必要があります。 ただし、一部のレガシープロジェクトでは、この方法が使用される場合があります。  詳しくは、[developer.adobe.com](https://developer.adobe.com/commerce/php/development/build/component-name/#add-a-modulexml-file){target="_blank"}を参照してください。\
>注意：このサンプルコードが機能するには、setup_versionを含める必要があります。そうしないと、InstallData.phpが実行されません。



```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="Learning_ClothingMaterial" setup_version="0.0.1"/>
</config>
```

### app/code/Learning/ClothingMaterial/Model/Attribute/Backend/Material.php

>[!NOTE]
>
>プロジェクト内の属性セット IDを使用してください。この例では、番号9です。

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Model\Attribute\Backend;

use Magento\Catalog\Model\Product;
use Magento\Eav\Model\Entity\Attribute\Backend\AbstractBackend;
use Magento\Framework\Exception\LocalizedException;

class Material extends AbstractBackend
{
    /**
     * @param Product @object
     * @throws LocalizedException
     */
    public function validate($object)
    {
        $value =$object->getData($this->getAttribute()->getAttributeCode());
        // Be sure to validate that your ID number it is likely to be different

        if (($object->getAttributeSetId() == 9) && ($value == 'fur')) {
            throw new LocalizedException(__('Bottoms cannot be fur'));
        }
    }
}
```

### app/code/Learning/ClothingMaterial/Model/Attribute/Frontend/Material.php

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Model\Attribute\Frontend;

use Magento\Eav\Model\Entity\Attribute\Frontend\AbstractFrontend;
use Magento\Framework\DataObject;

class Material extends AbstractFrontend
{

    public function getValue(DataObject $object): string
    {
        $value = $object->getData($this->getAttribute()->getAttributeCode());
        return "<b>$value</b>";
    }

}
```

### app/code/Learning/ClothingMaterial/Model/Attribute/Source/Material.php

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Model\Attribute\Source;

use Magento\Eav\Model\Entity\Attribute\Source\AbstractSource;

class Material extends AbstractSource
{
    /**
     * Get all options
     *
     * @retrun array
     */
    public function getAllOptions(): array
    {
       if(!$this->_options){
           $this->_options = [
               ['label'    => __('Cotton'),     'value' => 'cotton'],
               ['label'    => __('Leather'),    'value' => 'leather'],
               ['label'    => __('Silk'),       'value' => 'silk'],
               ['label'    => __('Fur'),        'value' => 'fur'],
               ['label'    => __('Wool'),       'value' => 'wool'],
           ];
       }
       return $this->_options;
    }
}
```

### app/code/Learning/ClothingMaterial/Setup/InstallData.php

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Setup;

use Learning\ClothingMaterial\Model\Attribute\Frontend\Material as Frontend;
use Learning\ClothingMaterial\Model\Attribute\Source\Material as Source;
use Learning\ClothingMaterial\Model\Attribute\Backend\Material as Backend;
use Magento\Catalog\Model\Product;
use Magento\Eav\Model\Entity\Attribute\ScopedAttributeInterface;
use Magento\Framework\Setup\InstallDataInterface;
use Magento\Framework\Setup\ModuleContextInterface;
use Magento\Framework\Setup\ModuleDataSetupInterface;

class InstallData implements InstallDataInterface
{
    protected $eavSetupFactory;

    public function __construct(
        \Magento\Eav\Setup\EavSetupFactory $eavSetupFactory
    )
    {
        $this->eavSetupFactory = $eavSetupFactory;
    }

    /**
     * @param ModuleDataSetupInterface $setup
     * @param ModuleContextInterface $context
     * {@inheritDoc}
     *
     * @SuppressWarnings(PHPMD.CyclomaticComplexity)
     * @suppressWarnings(PHPMD.ExessiveMethodLength)
     * @SuppressWarnings(PHPMD.NPathComplexity)
     */
    public function install(ModuleDataSetupInterface $setup, ModuleContextInterface $context)
    {
        $eavSetup = $this->eavSetupFactory->create();
        $eavSetup->addAttribute(
            Product::ENTITY,
            'clothing_material',
            [
                'group'         => 'Product Details',
                'type'          => 'varchar',
                'label'         => 'Clothing Material',
                'input'         => 'select',
                'source'        => Source::class,
                'frontend'      => Frontend::class,
                'backend'       => Backend::class,
                'required'      => false,
                'sort_order'    => 50,
                'global'        => ScopedAttributeInterface::SCOPE_GLOBAL,
                'is_used_in_grid'               => false,
                'is_visible_in_grid'            => false,
                'is_filterable_in_grid'         => false,
                'visible'                       => true,
                'is_html_allowed_on_frontend'   => true,
                'visible_on_front'              => true,
            ]
        );
    }
}
```

## 役立つリソース

[&#x200B; カスタムテキストフィールド属性を追加](https://developer.adobe.com/commerce/php/tutorials/admin/custom-text-field-attribute/)

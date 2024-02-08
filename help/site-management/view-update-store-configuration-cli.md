---
title: コマンドラインを使用して管理設定を表示および設定する
description: コマンドラインを使用して管理設定を表示および設定する方法について説明します。
feature: Configuration,Console,System
topic: Administration,Commerce
role: Developer
level: Beginner
doc-type: Technical Video
duration: 462
last-substantial-update: 2024-01-31T00:00:00Z
jira: KT-14877
thumbnail: KT-14877.jpeg
source-git-commit: a5ddf7591519b89efa2feb20ae601d36f5e5a1a7
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%

---


# コマンドラインを使用して管理設定を表示および設定する

Commerce CLI を使用して設定値を表示、設定、および検索する方法のデモです。 値が保存される場所とデフォルト値の保存元を把握します。

## このビデオは誰のためのものですか？

- Adobe Commerce開発者

## ビデオコンテンツ

>[!VIDEO](https://video.tv.adobe.com/v/3427123?&learn=on)

## チュートリアルで使用する一部のコマンド

パスワードセキュリティ設定を推奨に変更します。

`$ php bin/magento config:set admin/security/password_is_forced 0`

販売注文の自動コピー機能の電子メールアドレスを表示

`$ php bin/magento config:show sales_email/order/copy_to`

管理者に値を持つ設定の空の結果を表示

`php bin/magento config:show trans_email/ident_sales/email`

## チュートリアルで使用する Mysql クエリ

```
SELECT * FROM core_config_data WHERE path = 'sales_email/order/copy_to';

SELECT * FROM core_config_data WHERE path = 'sales_email/order_comment/copy_to';

SELECT * FROM core_config_data WHERE path = 'trans_email/ident_sales/email';
```

## デフォルトのセールスメールを検索する場所

コードベース内のどこかで定義されている設定値を見つける方法は？
`grep -rnw vendor/magento/ -e 'sales@example.com'`

端末でページを表示し、行番号を表示するには `cat -n vendor/magento/module-email/etc/config.xml`

## その他のリソース

- [コマンドラインツール](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/config-cli.html){target="_blank"}
- [管理者セキュリティの設定](https://experienceleague.adobe.com/docs/commerce-admin/systems/security/security-admin.html){target="_blank"}

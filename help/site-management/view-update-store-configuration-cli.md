---
title: コマンドラインを使用した管理設定の表示と設定
description: コマンドラインを使用して管理設定を表示および設定する方法を説明します。
feature: Configuration,Console,System
topic: Administration,Commerce
role: Developer
level: Beginner
doc-type: Technical Video
duration: 462
last-substantial-update: 2024-01-31T00:00:00Z
jira: KT-14877
thumbnail: KT-14877.jpeg
exl-id: 6cecba51-8d39-46f5-9864-80126d8ca3da
source-git-commit: 48a98261a827741459e45f14f7463f4a989c49d2
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%

---

# コマンドラインを使用した管理設定の表示と設定

Commerce CLI で設定値を表示、設定および検索する方法を示すデモ。 値が保存される場所と、デフォルト値の取得元を理解します。

## このビデオの目的は誰ですか。

- Adobe Commerce開発者

## ビデオコンテンツ

>[!VIDEO](https://video.tv.adobe.com/v/3427123?&learn=on)

## このチュートリアルで使用するコマンド

パスワードセキュリティ設定を推奨に変更します。

`$ php bin/magento config:set admin/security/password_is_forced 0`

販売注文自動コピー機能の E メール アドレスを表示します

`$ php bin/magento config:show sales_email/order/copy_to`

管理者に値を持つ設定の空の結果を表示します

`php bin/magento config:show trans_email/ident_sales/email`

## チュートリアルで使用する Mysql クエリ

```
SELECT * FROM core_config_data WHERE path = 'sales_email/order/copy_to';

SELECT * FROM core_config_data WHERE path = 'sales_email/order_comment/copy_to';

SELECT * FROM core_config_data WHERE path = 'trans_email/ident_sales/email';
```

## デフォルトの販売メールの場所

コードベースのどこかで定義されている設定値を検索するにはどうすればよいですか？
`grep -rnw vendor/magento/ -e 'sales@example.com'`

ページをターミナルで表示して行番号を表示するには、`cat -n vendor/magento/module-email/etc/config.xml` 下の手順に従います。

## その他のリソース

- [ コマンドラインツール ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/config-cli.html){target="_blank"}
- [Admin Security の設定 ](https://experienceleague.adobe.com/docs/commerce-admin/systems/security/security-admin.html){target="_blank"}

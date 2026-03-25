---
title: コマンドラインを使用した管理者設定の表示と設定
description: コマンドラインを使用して管理者設定を表示および設定する方法について説明します。
feature: Configuration,Console,System
topic: Administration,Commerce
role: Developer
level: Beginner
doc-type: Technical Video
duration: 510
last-substantial-update: 2024-01-31T00:00:00Z
jira: KT-14877
exl-id: 6cecba51-8d39-46f5-9864-80126d8ca3da
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%

---

# コマンドラインを使用した管理者設定の表示と設定

Commerce CLIで設定値を表示、設定、検索する方法のデモ。 値の保存場所とデフォルト値の保存場所を理解します。

## この動画は誰のためのものでしょうか？

* Adobe Commerce開発者

## ビデオコンテンツ

>[!VIDEO](https://video.tv.adobe.com/v/3427123?learn=on)

## チュートリアルで使用されるいくつかのコマンド

パスワードのセキュリティ設定を「推奨」に変更します。

`$ php bin/magento config:set admin/security/password_is_forced 0`

販売注文の自動コピー機能の電子メールアドレスを表示します

`$ php bin/magento config:show sales_email/order/copy_to`

管理者に値を持つ設定の空の結果を表示

`php bin/magento config:show trans_email/ident_sales/email`

## チュートリアルで使用するMysql クエリ

```
SELECT * FROM core_config_data WHERE path = 'sales_email/order/copy_to';

SELECT * FROM core_config_data WHERE path = 'sales_email/order_comment/copy_to';

SELECT * FROM core_config_data WHERE path = 'trans_email/ident_sales/email';
```

## デフォルトの販売メールの検索場所

コードベースのどこかに定義されている設定値を見つけるにはどうすればよいですか？
`grep -rnw vendor/magento/ -e 'sales@example.com'`

ターミナルでページを表示し、行番号`cat -n vendor/magento/module-email/etc/config.xml`を表示するには

## 関連資料

* [ コマンドラインツール ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/config-cli.html){target="_blank"}
* [管理者セキュリティの設定](https://experienceleague.adobe.com/docs/commerce-admin/systems/security/security-admin.html){target="_blank"}

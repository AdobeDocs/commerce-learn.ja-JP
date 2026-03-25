---
title: CLIを使用した管理者URIのリセット
description: Adobe Commerce Cloud CLIで管理者URIをリセットする方法について説明します。 この方法は、管理者URLの変更がアクセスの問題を引き起こす場合に便利です。
feature: Admin Workspace, Console
topic: Administration, Commerce
role: Developer, User
level: Beginner
doc-type: Technical Video
duration: 144
last-substantial-update: 2024-10-14T00:00:00Z
jira: KT-16338
exl-id: dbc155d7-8ce9-4622-abfb-1d8077c3a975
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '107'
ht-degree: 0%

---

# cliを使用して管理者URIをリセットする

Adobe Commerce Cloud cli コマンドを使用して管理者URIをリセットする方法について説明します。 これは、管理者のURLが管理者から変更されたものの、間違いがあり、管理者にアクセスできなくなった場合に便利です。

>[!VIDEO](https://video.tv.adobe.com/v/3435066?learn=on)

## チュートリアルで使用されるいくつかのコマンド

カスタム管理者パス URLを0に期待するように設定を変更します。

`$ php bin/magento config:set admin/url/use_custom 0`

カスタム管理パス URLの設定を0に変更します

`$ php bin/magento config:set admin/url/use_custom_path 0`

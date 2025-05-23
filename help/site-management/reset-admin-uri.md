---
title: CLI を使用して管理者 URI をリセットする
description: Adobe Commerce Cloud CLI で管理 URI をリセットする方法を説明します。 この方法は、管理者 URL の変更によってアクセスの問題が発生する場合に便利です。
feature: Admin Workspace, Console
topic: Administration, Commerce
role: Developer, User
level: Beginner
doc-type: Technical Video
duration: 123
last-substantial-update: 2024-10-14T00:00:00Z
jira: KT-16338
exl-id: dbc155d7-8ce9-4622-abfb-1d8077c3a975
source-git-commit: 25ee35b730cc6265665a87c9c37d24e88c41b60e
workflow-type: tm+mt
source-wordcount: '107'
ht-degree: 0%

---

# Cli を使用して管理者 URI をリセットする

Adobe Commerce Cloud cli コマンドを使用して管理者 URI をリセットする方法を説明します。 これは、管理者 URL が管理者から変更されたにもかかわらず、誤って管理者にアクセスできなくなっている場合に役立ちます。

>[!VIDEO](https://video.tv.adobe.com/v/3439693/?learn=on&captions=jpn)

## このチュートリアルで使用するコマンド

カスタム管理パス URL を 0 に設定するように変更します。

`$ php bin/magento config:set admin/url/use_custom 0`

カスタム管理パス URL の設定を 0 に変更します

`$ php bin/magento config:set admin/url/use_custom_path 0`

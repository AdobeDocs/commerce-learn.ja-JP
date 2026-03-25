---
title: 設定値
description: core_config_data、XML ファイル、および管理者設定を使用して、Adobe Commerceで設定値を検索、検証、および管理する方法について説明します。
feature: Cloud, Configuration, System, Variables
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Beginner
doc-type: Technical Video
duration: 709
last-substantial-update: 2024-11-08T00:00:00Z
jira: KT-16429
exl-id: 9ff16e96-a63f-4fab-be7d-9160c1172603
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 0%

---

# 設定値

このガイドでは、Adobe Commerceで設定値を検索、検証、管理する方法について包括的に説明します。 core_config_data テーブルのクエリやCLI コマンドの使用などの基本的な方法、およびXML ファイルや環境固有の設定に関する高度なテクニックについて説明します。 さまざまな環境をまたいで一貫性を維持するためのベストプラクティスと、環境変数の設定にAdobe Commerce Cloud管理UI や.magento.app.yamlなどのツールを使用する方法について説明します。

## このビデオの対象

* Adobe CommerceとAdobe Commerce Cloudを使用する開発者
* システム管理者またはテクニカルサポートスタッフ（サーバーレベルでのサポートを求められる可能性がある）

## ビデオコンテンツ

* core_config_data テーブル、XML ファイル、およびCLI コマンドを使用して、Adobe Commerceで設定値を検索する方法を説明します。
* 管理者パネル、コードリポジトリ、環境固有のファイルを使用して、設定値を編集および上書きする方法について説明します。
* app:config:dumpやAdobe Commerce Cloudの管理UIなどのツールを使用して、様々な環境で一貫した設定値を維持するためのベストプラクティスをご紹介します。

>[!VIDEO](https://video.tv.adobe.com/v/3436458?learn=on)

## 関連するExperience League ドキュメント

* [設定の書き出し](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/configuration-management/export-configuration)
* [構成設定を上書き](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/paths/override-config-settings)
* [設定値を設定](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/configuration-management/set-configuration-values)
* [Config参照config.php](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/files/config-reference-configphp)
* [設定ガイドの技術的な詳細](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/deployment/technical-details)
* [ ロックされた設定値](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/deployment/technical-details#:~:text=Configuration%20settings%20locked%20in%20the,php%20files)
* [env.phpに値を保存](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/locked-fields-in-magento-admin#:~:text=Cause,php%20)

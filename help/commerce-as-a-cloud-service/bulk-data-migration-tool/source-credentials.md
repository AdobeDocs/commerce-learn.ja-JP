---
title: Bulk Data Migration Tool - Source Credentials
description: 一括データ移行ツールを実行する前に、.env ファイルでソースインスタンス URLと認証資格情報を設定する方法を説明します。
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 238
last-substantial-update: 2026-07-21T00:00:00.000Z
jira: KT-22095
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 601e4abe-d9bf-58de-a779-32ed6794dcbe
    internal-label: Data Import/Export
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 0%
---
# 一括データ移行ツールのソース資格情報の設定

一括データ移行ツールを実行する前に、`.env` ファイルでソースインスタンス URLと認証資格情報を設定します。 認証手順は、ソース環境がオンプレミスであるかAdobe Commerce as a Cloud Service（PaaS）であるかによって少し異なります。

## この動画は誰のためのものでしょうか？

* ソリューションアーキテクト
* DevOps エンジニア
* バックエンド開発者

## ビデオコンテンツ

* ソースインスタンス URLと、RESTおよびGraphQLのURLを`.env` ファイルに設定します。
* Adobe Commerce Adminで&#x200B;**System** > **Extensions** > **Integrations**&#x200B;から統合キーを取得または作成します。
* 必要な4つのトークンを生成するには、統合をアクティブにします。
* ソースがAdobe Commerce as a Cloud Service（PaaS）の場合は、account.magento.cloudからMagento CLI トークンを取得します。

>[!VIDEO](https://video.tv.adobe.com/v/3496142?learn=on)

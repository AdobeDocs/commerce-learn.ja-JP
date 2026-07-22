---
title: Bulk Data Migration Tool - Source Credentials
description: 一括データ移行ツールを実行する前に、.env ファイルでソースインスタンス URLと認証資格情報を設定する方法を説明します。
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 238
last-substantial-update: 2026-07-21T00:00:00Z
jira: KT-22095
source-git-commit: a785518a36cda9d2bfb82951c26f2e197ee3d43e
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

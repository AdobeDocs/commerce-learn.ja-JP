---
title: バルクデータ移行ツール - DB資格情報
description: 移行ツールを実行する前に、Magento Cloud CLIまたはプロジェクト IDを使用して、.my.cnf ファイルでソースデータベース接続を設定する方法を説明します。
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 161
last-substantial-update: 2026-07-21T00:00:00.000Z
jira: KT-22105
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
source-wordcount: '151'
ht-degree: 0%
---
# 一括データ移行ツールのデータベース資格情報の設定

一括データ移行ツールを実行する前に、`.my.cnf` ファイルでソースデータベース接続を設定します。 手順は、ソース環境がオンプレミスであるか、Adobe Commerce as a Cloud Service（PaaS）であるかによって異なります。

## この動画は誰のためのものでしょうか？

* ソリューションアーキテクト
* DevOps エンジニア
* バックエンド開発者

## ビデオコンテンツ

* `.my.cnf.example`を`.my.cnf`にコピーし、ソース接続用にという名前の新しいセクションを作成します。
* ソースがAdobe Commerce as a Cloud Service （PaaS）の場合、プロジェクト IDを`.my.cnf`に設定します。
* Magento Cloud CLIのトンネルコマンドを使用して、ホスト、ユーザー、パスワード、ポート、データベースの値を取得します。
* ソースがオンプレミスの場合は、ツールを実行する前に、ホストとポートの接続を確認してください。

>[!VIDEO](https://video.tv.adobe.com/v/3496152?learn=on)

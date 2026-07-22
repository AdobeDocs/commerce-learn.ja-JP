---
title: バルクデータ移行ツール - DB資格情報
description: 移行ツールを実行する前に、Magento Cloud CLIまたはプロジェクト IDを使用して、.my.cnf ファイルでソースデータベース接続を設定する方法を説明します。
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 161
last-substantial-update: 2026-07-21T00:00:00Z
jira: KT-22105
source-git-commit: 0dcb41e9138a36528f10333b0b5a9a9b2a39ed40
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
* Magento Cloud CLI トンネルコマンドを使用して、host、user、password、portおよびdatabaseの値を取得します。
* ソースがオンプレミスの場合は、ツールを実行する前に、ホストとポートの接続を確認してください。

>[!VIDEO](https://video.tv.adobe.com/v/3496152?learn=on)

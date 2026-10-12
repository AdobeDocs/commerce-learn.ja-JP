---
title: バルクデータ移行ツール – ターゲット資格情報
description: 一括データ移行ツールを実行する前に、.env ファイルでターゲットインスタンスのURL、Adobe IMS資格情報、CDMS設定を行う方法について説明します。
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 226
last-substantial-update: 2026-07-21T00:00:00.000Z
jira: KT-22107
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
source-wordcount: '173'
ht-degree: 0%
---
# 一括データ移行ツールのターゲット資格情報の設定

一括データ移行ツールを実行する前に、`.env` ファイルでターゲットインスタンスのURL、Adobe IMS資格情報、CDMS設定を設定します。 Adobe IMSURL、ターゲット URL、CDMSが、ステージや実稼動など、同じ環境層に対応していることを確認します。

## この動画は誰のためのものでしょうか？

* ソリューションアーキテクト
* DevOps エンジニア
* バックエンド開発者

## ビデオコンテンツ

* experience.adobe.comのインスタンス情報パネルの値を使用して、`.env` ファイルのターゲットインスタンス REST URLとGraphQL URLおよびターゲットテナント IDを設定します。
* Adobe IMS層（ステージまたは実稼動）とリージョンに合わせて環境URLを設定します。
* Adobe Developer Consoleの&#x200B;**Project** > **OAuth Server-to-Server**&#x200B;からAdobe IMSクライアント IDとクライアントシークレットを取得します。
* ターゲット組織IDをコピーし、環境に合わせてCDMS ホスト、ポート、ローカルサーバーの設定を設定します。

>[!VIDEO](https://video.tv.adobe.com/v/3496167?learn=on)

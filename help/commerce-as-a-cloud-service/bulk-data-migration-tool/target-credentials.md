---
title: バルクデータ移行ツール – ターゲット資格情報
description: 一括データ移行ツールを実行する前に、.env ファイルでターゲットインスタンスのURL、Adobe IMS資格情報、CDMS設定を行う方法について説明します。
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 226
last-substantial-update: 2026-07-21T00:00:00Z
jira: KT-22107
source-git-commit: b3c029f7c1080550900cbc5838478cd7a4137a20
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

>[!VIDEO](https://video.tv.adobe.com/v/3496168?captions=jpn&learn=on)

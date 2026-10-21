---
title: MySQL スロークエリログでのGalera DB レプリケーションの診断
description: Galera DBのレプリケーション設計がセカンダリデータベースの同期を遅くする方法、MySQLのスロークエリログでこれらのイベントを特定する方法、および影響を最小限に抑える方法について説明します。
doc-type: Technical Video
duration: 452
last-substantial-update: 2023-07-18T00:00:00.000Z
feature: Backend Development, Logs, Services
topic: Commerce, Development
role: Developer
level: Intermediate
jira: KT-13635
exl-id: 4a8a2df1-8cac-4bd9-851f-0eaae011b76c
TQID: 'https://experienceleague.adobe.com/NYapiIjnRv5RAS1glm8do16M4jUPmbgfVCs6ICQwbUc'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: b48dbafb-4193-5648-b9d9-bf96e9c9a411
    internal-label: Backend Development
  - id: da76473c-f99b-5ad0-9b14-896aed473f8a
    internal-label: Services
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
subfeature_v2:
  - id: 3c398179-d35a-51ba-b317-6c5b95feef5e
    internal-label: Logs
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
source-git-commit: 7516911fc14bc80f327310b99a00f9cbff7c65c7
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%
---
# Galera DB レプリケーションと関連するMySQL スロークエリについて説明します

Galera クラスターはパフォーマンスと拡張性に役立ちます。 レプリカ・データベースを検討する場合、データ・レプリケーションの実行方法がプライマリ・データベースとは異なることを理解することが重要です。 プライマリデータベースは一括操作を実行できます。 すべてのレプリカデータベースに対してレプリケーションが実行されると、アクションは一度に1つずつ実行されます。 例えば、削除内に67,000,000個の項目がある場合、レプリカデータベース上では、各項目が一度に1つずつ発生します。 MySQLのスロークエリログを確認すると、このアクションに時間がかかる場合があります。 レプリカデータベースが順次動作していることは、同期しない理由であり、パフォーマンスへの影響を検出できます。

レプリカデータベースをプライマリと同期させるには、可能な限り大規模な操作をバッチ処理します。 バッチ処理により、アクションをタイムリーに実行し、パフォーマンスへの影響を最小限に抑えることができます。

## 対象オーディエンス

* 設計者
* 開発者
* DevOps

## ビデオコンテンツ

* レプリカデータベースへのガレラレプリケーション
* フロー制御について詳しく見る
* mysql スロークエリログでのスレッド番号の検索
* 一括実行はプライマリでのみ実行されます。 レプリケーションは1回に1回ずつ実行されます
* レプリケーションがプライマリに対応できるように、大規模なコミットをバッチ処理します。

>[!VIDEO](https://video.tv.adobe.com/v/3423542?captions=jpn&learn=on)

## 役立つリソース

* [ガレラ群](https://mariadb.com/products/enterprise/galera-cluster/)

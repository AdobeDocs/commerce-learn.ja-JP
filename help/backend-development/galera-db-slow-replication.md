---
title: MySQL スロークエリログでのGalera DB レプリケーションの診断
description: Galera DBのレプリケーション設計がセカンダリデータベースの同期を遅くする方法、MySQLのスロークエリログでこれらのイベントを特定する方法、および影響を最小限に抑える方法について説明します。
doc-type: Technical Video
duration: 452
last-substantial-update: 2023-07-18
feature: Backend Development, Logs, Services
topic: Commerce, Development
role: Developer
level: Intermediate
jira: KT-13635
exl-id: 4a8a2df1-8cac-4bd9-851f-0eaae011b76c
TQID: https://experienceleague.adobe.com/NYapiIjnRv5RAS1glm8do16M4jUPmbgfVCs6ICQwbUc
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
source-git-commit: add3e29f8841ca4ca99f4c40afc656f00e93ec36
workflow-type: tm+mt
source-wordcount: 262
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

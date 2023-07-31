---
title: mysql クエリのキャッシュ方法を説明します
description: 時には、mysql クエリがロックを待ってバックアップされます。 このチュートリアルでは、クエリキャッシュとは何か、および問題がある場合の設定に関する推奨事項を説明します。
kt: 13690
doc-type: video
activity: use
last-substantial-update: 2023-7-27
feature: Backend Development, Cache, Logs
topic: Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: d62f409324eda53ca42cf2e9c9aa80655a1277d7
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 0%

---

# mysql クエリのキャッシュについて説明します

MySQL クエリキャッシュの概要と、その動作の基本的な理解について説明します。 mysql のクエリキャッシュの問題を検出する方法を説明します。この方法を使用すると、mysql の低速クエリログで、「クエリキャッシュのロックを待機中」が大量に表示されます。

## このビデオは誰のためのものですか？

- アーキテクト
- 開発者
- DevOps

## ビデオコンテンツ

- クエリのキャッシュについて説明します
- 「クエリキャッシュのロックを待機中」を見つけて、クエリキャッシュの設定が問題になる可能性があるかどうかを検出する方法
- SQL が保存され、一致するクエリキャッシュの検索で使用される仕組みを確認する
- 設定に関するヒント

>[!VIDEO](https://video.tv.adobe.com/v/3422015?learn=on)

## 役に立つリソース

- [一般的な MySQL ガイドライン](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/database-server/mysql.html?lang=en){target="_blank"}
- [MySQL のデッドロック](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/database/deadlocks-in-mysql.html){target="_blank"}
- [Galera レプリケーションと処理の遅いクエリ](https://experienceleague.adobe.com/docs/commerce-learn/tutorials/backend-development/galera-db-slow-replication.html){target="_blank"}

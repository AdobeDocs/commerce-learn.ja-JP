---
title: Percona Toolkit pt-query-digest の仕組みと使用理由を説明します。
description: MySQL クエリを、低速、一般、バイナリのログファイルから分析します。 また、「SHOW PROCESSLIST」と tcpdump の MySQL プロトコルデータからクエリを分析することもできます。
kt: 13846
doc-type: video
activity: use
last-substantial-update: 2023-8-28
feature: Backend Development, Tools and External Services, Logs
topic: Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: fdb908f6085b2f050d52742d22241093b46415ed
workflow-type: tm+mt
source-wordcount: '114'
ht-degree: 0%

---

# Percona Toolkit pt-query-digest の詳細

pt-query-digest といくつかの実際の例を使用して、推論をより深くおこなう理由を説明します。

## このビデオは誰のためのものですか？

- アーキテクト
- 開発者
- DevOps

## ビデオコンテンツ

- pt-query-digest の使用について説明します。
- この Percona Toolkit 機能の利点と欠点について説明します。
- 結果を理解し、考えられるパフォーマンスステップを考慮する必要があるかを学びます。

>[!VIDEO](https://video.tv.adobe.com/v/3423480?learn=on)

## コード参照

```MYSQL
Be sure to change to match your logs and time frame

$ pt-query-digest mysql-slow.log.7 > mysql-slow.log.7.DIGEST
```

## 役に立つリソース

- [Percona Toolkit](https://docs.percona.com/percona-toolkit/pt-query-digest.html){target="_blank"}
- [MySQL のデッドロック](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/database/deadlocks-in-mysql.html){target="_blank"}

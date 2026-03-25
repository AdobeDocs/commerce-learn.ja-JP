---
title: Percona Toolkit pt-query-digestの仕組みと使用理由について説明します
description: スローログファイル、一般ログファイル、バイナリログファイルからMySQL クエリを分析します。 また、「SHOW PROCESSLIST」からのクエリや、tcpdumpからのMySQL プロトコルデータを分析することもできます。
kt: 13846
doc-type: video
duration: 510
activity: use
last-substantial-update: 2023-8-28
feature: Backend Development, Tools and External Services, Logs
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: 77e91f1b-b3ae-4c6d-bb6d-4fd7ebbb0baf
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '98'
ht-degree: 0%

---

# Percona Toolkit pt-query-digest

pt-query-digestと実例を使用して推論を深める理由を説明します。

## この動画は誰のためのものでしょうか？

* 設計者
* 開発者
* DevOps

## ビデオコンテンツ

* pt-query-digestの使用方法について説明します
* このPercona Toolkit機能のメリットと欠点について説明します
* 結果を把握し、考慮すべきパフォーマンスのステップを把握します

>[!VIDEO](https://video.tv.adobe.com/v/3423480?learn=on)

## コード参照

```MYSQL
Be sure to change to match your logs and time frame

$ pt-query-digest mysql-slow.log.7 > mysql-slow.log.7.DIGEST
```

## 役立つリソース

* [Percona Toolkit](https://docs.percona.com/percona-toolkit/pt-query-digest.html){target="_blank"}

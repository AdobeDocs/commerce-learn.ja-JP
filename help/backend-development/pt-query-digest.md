---
title: Percona Toolkit pt-query-digest の仕組みと使用される理由を説明します
description: 低速のログファイル、一般的なログファイル、バイナリログファイルから MySQL クエリを分析します。 また、「SHOW PROCESSLIST」からのクエリと、tcpdump からの MySQL プロトコルデータも分析できます。
kt: 13846
doc-type: video
activity: use
last-substantial-update: 2023-8-28
feature: Backend Development, Tools and External Services, Logs
topic: Commerce, Development
role: Architect, Developer
level: Intermediate
exl-id: 77e91f1b-b3ae-4c6d-bb6d-4fd7ebbb0baf
source-git-commit: a2d644de420f9188be108fad36ae97dfbf1a75eb
workflow-type: tm+mt
source-wordcount: '98'
ht-degree: 0%

---

# Percona Toolkit pt-query-digest

pt-query-digest と実際の例を使用して、推論を深める理由を説明します。

## このビデオの目的は誰ですか。

- 建築士
- 開発者
- DevOps

## ビデオコンテンツ

- pt-query-digest usage について説明します
- この Percona Toolkit 機能のメリットと欠点を説明します
- 結果を理解し、考えられるパフォーマンス手順を学びます

>[!VIDEO](https://video.tv.adobe.com/v/3452288?learn=on&captions=jpn)

## コード参照

```MYSQL
Be sure to change to match your logs and time frame

$ pt-query-digest mysql-slow.log.7 > mysql-slow.log.7.DIGEST
```

## 役に立つリソース

- [Percona Toolkit](https://docs.percona.com/percona-toolkit/pt-query-digest.html){target="_blank"}

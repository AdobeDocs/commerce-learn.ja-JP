---
title: Commerce Cloudの一般的なエラーをいくつか診断して修正します
description: サイトの読み込みを妨げる2つの一般的なAdobe Cloud プロジェクトエラーを解決します。
feature: Cloud, Site Management
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 297
last-substantial-update: 2024-10-30T00:00:00Z
jira: KT-16419
exl-id: 4c21b6a6-783a-422f-9071-3534ed68e8be
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---

# サービスが利用できないため、エラーが発生しました

Adobe Commerce Cloud プロジェクトで発生する2つの一般的なエラーをトリアージして解決する方法について説明します。  これらのエラーがどのように、なぜ発生するのか、またそれらを解決するために推奨される手順は何かを理解します。

## このビデオの対象

* 開発者およびIT担当者
* システム管理者

## ビデオコンテンツ

* ストレージの問題の診断と解決：
* メンテナンスモードの管理
* 効率的なトラブルシューティングのヒント

>[!VIDEO](https://video.tv.adobe.com/v/3435766?learn=on)


## ビデオで使用されるコマンド

応答メッセージに記載されている例外ログの最後の5行を検索します。

```SHELL
 tail -n 5 ~/var/log/exception.log
```

ハードディスクの空き容量を確認します。 dev/mapper/xxxx行に注意してください

```SHELL
df -h
```

上位15個のファイルを検索できます

```SHELL
find -type f -exec du -Sh {} + | sort -rh | head -n 15
```

メンテナンスモードのステータスの表示

```SHELL
php bin/magento maintenance:status
```

メンテナンスモードを無効にする

```SHELL
php bin/magento maintenance:disable 
```

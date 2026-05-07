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
last-substantial-update: 2024-10-30T00:00:00.000Z
jira: KT-16419
exl-id: 4c21b6a6-783a-422f-9071-3534ed68e8be
TQID: https://experienceleague.adobe.com/-mN2UoNU6uKjUoHmZT59LgT4o7p4d4O2Zl1BR3x8y-8
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 135
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

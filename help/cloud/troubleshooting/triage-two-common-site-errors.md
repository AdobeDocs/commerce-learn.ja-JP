---
title: 一般的なCommerce Cloud エラーをいくつか診断して修正します
description: サイトの読み込みを妨げる 2 つの一般的なAdobe Cloud プロジェクトエラーを解決します。
feature: Cloud, Site Management
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 260
last-substantial-update: 2024-10-30T00:00:00Z
jira: KT-16419
exl-id: 4c21b6a6-783a-422f-9071-3534ed68e8be
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---

# 診断および修正サービスを利用できず、エラーが発生しました

Adobe Commerce Cloud プロジェクトで発生する 2 つの一般的なエラーをトリアージして解決する方法を説明します。  これらのエラーが発生する方法と理由、およびエラーを解決するための推奨される手順を理解します。

## このビデオの対象

- 開発者と IT 担当者
- システム管理者

## ビデオコンテンツ

- ストレージの問題を診断および解決：
- メンテナンスモードの管理
- 効率的なトラブルシューティングのヒント

>[!VIDEO](https://video.tv.adobe.com/v/3447694?captions=jpn&learn=on)


## ビデオで使用されているコマンド

応答メッセージに記載されている例外ログの最後の 5 行を確認します。

```SHELL
 tail -n 5 ~/var/log/exception.log
```

ハード ドライブの空き容量を確認するには dev/mapper/xxxx という行に注意してください。

```SHELL
df -h
```

上位 15 個の最大ファイルを検索できます

```SHELL
find -type f -exec du -Sh {} + | sort -rh | head -n 15
```

メンテナンスモードのステータスの表示

```SHELL
php bin/magento maintenance:status
```

メンテナンスモードの無効化

```SHELL
php bin/magento maintenance:disable 
```

---
title: 一般的なCommerce Cloudエラーをいくつか診断して修正する
description: サイトの読み込みを妨げる 2 つの一般的なAdobeクラウドプロジェクトエラーを解決します。
feature: Cloud, Site Management
topic: Commerce, Development
role: Architect, Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 260
last-substantial-update: 2024-10-30T00:00:00Z
jira: KT-16419
source-git-commit: 27c1715dd42853013181d9c729549a5a32bc2af0
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

>[!VIDEO](https://video.tv.adobe.com/v/3447694?learn=on&captions=jpn)


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

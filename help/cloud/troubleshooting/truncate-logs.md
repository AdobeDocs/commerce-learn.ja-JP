---
title: ログをトランケート
description: 大きなログファイルを切り捨てて、ハードドライブがいっぱいになったことが原因でデプロイメントが失敗した場合の対処方法を説明します。
feature: Cloud, Site Management
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 206
last-substantial-update: 2025-3-25
jira: KT-17595
exl-id: 4a36de40-fb55-41ad-afef-35fc18a271ec
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '187'
ht-degree: 0%

---

# ログをトランケート

ハードドライブ容量超過が原因でデプロイメントが失敗した場合のトリアージ方法を説明します。 Adobe Commerce クラウド環境の領域を解放するために実行できるコマンドを見つける方法と内容について説明します。

これらのログ ファイルが必要だと思われる場合は、切り捨てる前に、それらを `rsync` るか、他の方法を使用してサーバーからコピーを取得できます。

## このビデオの対象

- 開発者と IT 担当者
- システム管理者

## ビデオコンテンツ

- 失敗したデプロイメントの診断と解決
- 一般的な大きなログファイルの場所
- ログファイルをトランケートする簡単な方法

>[!VIDEO](https://video.tv.adobe.com/v/3454572?learn=on)


## ビデオで使用されているコマンド

ハードディスク（HDD）の空き容量を `df -h` しく確認します。 dev/mapper/xxxx という行に注意してください。

```SHELL
df -h

Filesystem                              Size  Used Avail Use% Mounted on
/dev/loop217                           1016M 1016M     0 100% /
none                                    492K  4.0K  488K   1% /dev
fake-sysfs                              120G     0  120G   0% /sys
tmpfs                                   120G     0  120G   0% /sys/fs/cgroup
tmpfs                                   384M     0  384M   0% /dev/shm
tmpfs                                    50M  460K   50M   1% /run
tmpfs                                   5.0M     0  5.0M   0% /run/lock
/dev/loop236                            144M  144M     0 100% /app
/dev/mapper/hyjh5nlaoabqtxxnh4opgjqzpu  4.9G  4.9G     0 100% /mnt
/dev/loop14                             8.0G  403M  7.6G   5% /tmp
/dev/mapper/platform-lxc                5.0T   69G  4.7T   2% /run/shared
```


コマンド `ls -lah` を使用して、ファイルとそのサイズを人間が読み取れる形式（kb、mb、gb など）で表示する

```SHELL
ls -lah

total 4.7G
drwxr-xr-x 2 web web 4.0K Jul 16  2024 .
drwxr-xr-x 6 web web 4.0K Jan 10  2024 ..
-rw-rw-r-- 1 web web 487K Jul  5  2024 cache.log
-rw-r--r-- 1 web web 1.2K Jul 16  2024 cloud.error.log
-rw-r--r-- 1 web web 328K Mar 25 14:09 cloud.log
-rw-rw-r-- 1 web web 2.4G Jul  5  2024 cron.log
-rw-rw-r-- 1 web web  363 Dec  6  2023 debug.log
-rw-rw-r-- 1 web web  15K Jan 10  2024 indexation.log
-rw-r--r-- 1 web web 229K Jan 10  2024 install_upgrade.log
-rw-r--r-- 1 web web 2.9K Jul 16  2024 patch.log
-rw-rw-r-- 1 web web 2.4G Mar 25 15:36 support_report.log
-rw-rw-r-- 1 web web  516 Dec  6  2023 system.log
```

## トランケート・ログの例

適切なプロジェクトと環境に ssh で接続したら、`var/log` ディレクトリに移動します。 次に、`> some-log-file.log` に似たものを使用してファイルを切り捨てることができます

```BASH
> support_report.log 
> cron.log 
```

## 関連ドキュメント

- [&#x200B; 正常性の通知 &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/integrations/health-notifications){target="_blank"}

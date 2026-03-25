---
title: ログを切り捨てる
description: 大きなログファイルを切り捨てることで、フルハードドライブが原因で失敗したデプロイメントをトリアージする方法を説明します。
feature: Cloud, Site Management
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 302
last-substantial-update: 2025-3-25
jira: KT-17595
exl-id: 4a36de40-fb55-41ad-afef-35fc18a271ec
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '187'
ht-degree: 0%

---

# ログを切り捨てる

フル ハード ドライブによるデプロイメントのトリアージと失敗の方法について説明します。 Adobe Commerce Cloud環境の空き容量を増やすために実行できるコマンドの検索方法と実行方法について説明します。

これらのログファイルが必要になる可能性がある場合は、それらを`rsync`するか、切り捨てる前にサーバーから利用可能なコピーを取得するために他の方法を使用できます。

## このビデオの対象

* 開発者およびIT担当者
* システム管理者

## ビデオコンテンツ

* 失敗したデプロイメントの診断と解決
* 一般的な大きなログファイルが見つかった場合
* ログファイルを切り捨てるクイックメソッド

>[!VIDEO](https://video.tv.adobe.com/v/3454572?learn=on)


## ビデオで使用されるコマンド

ハード ドライブの空き容量を確認するには`df -h`。 dev/mapper/xxxx行に注意してください

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


コマンド `ls -lah`を使用して、ファイルとそのサイズをkb、mb、gbなどの人間が読み取れる形式で表示します

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

## 切り捨てログの例

適切なプロジェクトと環境にSSHで移動したら、`var/log` ディレクトリに移動します。 次に、`> some-log-file.log`に似たものでファイルを切り捨てることができます

```BASH
> support_report.log 
> cron.log 
```

## 関連ドキュメント

* [&#x200B; ヘルス通知](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/integrations/health-notifications){target="_blank"}

---
title: データベースへのテーブルの追加
description: '[!DNL Commerce] には、データベーステーブルの作成、既存のテーブルの変更、データの追加を可能にする特別なメカニズムがあります。'
topic: Development
kt: 5613
doc-type: video
activity: use
exl-id: fb222752-5689-4f87-94cf-a61ed7005e6b
source-git-commit: 1eb2cd22f9bded77032ad0ed43c3f2ca84879a69
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 0%

---

# データベースへのテーブルの追加

[!DNL Commerce] には、データベーステーブルの作成、既存のテーブルの変更、データの追加を可能にする特別なメカニズムがあります。例えば、設定データは、モジュールのインストール時に追加する必要があります。このメカニズムにより、これらの変更を異なるインストール間で転送できます。

システムの再インストール時に手動で SQL 操作を繰り返し行うのではなく、開発者は、データを含むインストール（またはアップグレード）スクリプトを作成します。 このスクリプトは、モジュールがインストールされるたびに実行されます。

## このビデオは誰のためのものですか。

- 開発者

## ビデオコンテンツ

- モジュールの作成
- InstallSchema スクリプトの作成
- InstallData スクリプトの作成
- 新しいモジュールを追加し、データを含むテーブルが作成されたことを確認します
- UpgradeSchema スクリプトの作成
- UpgradeData スクリプトの作成
- アップグレードスクリプトを実行し、テーブルが変更されたことを確認します

>[!VIDEO](https://video.tv.adobe.com/v/35791?quality=12&learn=on)

## 役に立つリソース

- [移行コマンド](https://devdocs.magento.com/guides/v2.4/extension-dev-guide/declarative-schema/migration-commands.html)

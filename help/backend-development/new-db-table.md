---
title: データベースにテーブルを追加する
description: '"[!DNL Commerce] には、データベーステーブルの作成、既存のテーブルの変更、データの追加を可能にする特別なメカニズムがあります。'
kt: 5613
doc-type: video
activity: use
last-substantial-update: 2023-2-10
feature: Configuration, System, Backend Development
topic: Commerce, Development
role: Admin, Developer
level: Beginner, Intermediate
exl-id: fb222752-5689-4f87-94cf-a61ed7005e6b
source-git-commit: 79529c8d77df74e6f77ab3a01b45541a38dbf680
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# データベースにテーブルを追加する

>[!IMPORTANT]
>
>これはお勧めしません。 https://developer.adobe.com/commerce/php/development/components/declarative-schema/を参照してください。


[!DNL Commerce] には、データベーステーブルの作成、既存のテーブルの変更、データの追加を可能にする特別なメカニズムがあります。設定データは、モジュールのインストール時に追加する必要があります。 このメカニズムにより、これらの変更を異なるインストール間で転送できます。

システムの再インストール時に手動で SQL 操作を繰り返し行う代わりに、開発者はデータを含むインストール（またはアップグレード）スクリプトを作成します。 このスクリプトは、モジュールがインストールされるたびに実行されます。

## このビデオは誰のためのものですか？

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

- [インストール/アップグレードスクリプトを宣言スキーマに移行](https://developer.adobe.com/commerce/php/development/components/declarative-schema/migration-scripts/)

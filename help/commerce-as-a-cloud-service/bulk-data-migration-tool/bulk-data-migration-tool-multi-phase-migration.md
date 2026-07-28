---
title: バルクデータ移行ツール – 多相移行
description: 実稼動カットオーバー中にソースをフリーズしたままにする必要がある場合に、メンテナンスモードを使用して一括データ移行ツールを使用してマルチフェーズ移行を実行する方法を説明します。
feature: Data Import/Export
topic: Migration
role: Developer
doc-type: Technical Video
duration: 211
last-substantial-update: 2026-07-27T00:00:00Z
jira: KT-22157
source-git-commit: c3b81a5ffc652bc7ce7640b67fe5529067607251
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---


# 一括データ移行ツールを使用した多相移行の実行

抽出中にソース環境を凍結する必要がある場合に多段階の移行を実行します。移行の途中で新しい注文が届かない場合の実稼動上のカットオーバーに最適です。 メンテナンスモードを使用し、5つのフェーズを順番に実行する必要があります。 ソースがライブ状態を維持できる場合は、代わりにこのシリーズの単相移行ビデオを参照してください。

## この動画は誰のためのものでしょうか？

* ソリューションアーキテクト
* DevOps エンジニア
* バックエンド開発者

## ビデオコンテンツ

* 開始する前の1つの重要な違い：`bin/console` コマンドが移行ツール自体に対して実行され、`bin/magento maintenance` コマンドがソース Commerce サーバーで実行されます。 このツールは、メンテナンスモードを有効または無効にしません。これは手動の手順です。
* ソースがまだ稼動している間にフェーズ 1が実行されます。`bin console migration:before-maintenance`は設定を確認し、環境を初期化し、CDMSに接続し、移行を登録し、機能テストを実行し、合成テストデータを作成します。 このフェーズが完了するまで、メンテナンスモードを有効にしないでください。
* フェーズ 3は、凍結環境からの抽出です。`bin/console migration:during-maintenance`は、必要に応じてPaaS トンネルを再開し、ソースからの抽出、ステージングビューのクリーンアップ、ACCS ターゲットへの読み込み、検証の実行、ターゲット上のテストデータのクリーンアップを行います。

>[!VIDEO](https://video.tv.adobe.com/v/3496413?learn=on)

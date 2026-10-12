---
title: バルクデータ移行ツール – 単相移行
description: ドライ実行および抽出中にソースを維持できる環境に対して、一括データ移行ツールを使用して単相移行を実行する方法について説明します。
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 737
last-substantial-update: 2026-07-24T00:00:00.000Z
jira: KT-22139
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 601e4abe-d9bf-58de-a779-32ed6794dcbe
    internal-label: Data Import/Export
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%
---
# 一括データ移行ツールを使用した単相移行の実行

ソース環境が抽出中も稼働し続けることができる場合は、単相移行を実行します。これは、ドライ実行、開発またはサンドボックス環境に最適です。 移行の途中で新しい注文が来ない実稼動カットオーバーなど、固定されたソースが必要な場合は、代わりにこのシリーズの段階的な移行ビデオを参照してください。

## この動画は誰のためのものでしょうか？

* ソリューションアーキテクト
* DevOps エンジニア
* バックエンド開発者

## ビデオコンテンツ

* `bin console build`を使用してDocker イメージをビルドします。Dockerfileが変更された場合にのみ、これを再実行してください。
* CDMS CLI コンテナマネージャーを起動するには、`bin console start`を実行し、コンテナ内のシェルを1回開いて依存関係をダウンロードします。
* 完全な10 ステップのパイプラインを実行するには、`bin console migration`を実行します。設定の確認、環境の初期化、PaaS トンネルのオープン、統合テストの実行、CDMSへの登録、ターゲットスキーマの分析、テストデータの生成、ソースデータの抽出、ACCSへの読み込み、チェックサムの検証、クリーンアップ、および要約を行います。
* 移行の概要レポートを確認する – ステップ 8 （データの整合性の検証）は、パイプラインを停止せずに失敗をログに記録するため、完了した実行ではクリーンな検証が保証されません。
* この単一フェーズのコマンドは、完全な自己完結型パイプラインです。独自の専用コマンドを持つメンテナンスモード（段階的な移行）ワークフロー内のステップとして使用しないでください。

>[!VIDEO](https://video.tv.adobe.com/v/3496317?captions=jpn&learn=on)

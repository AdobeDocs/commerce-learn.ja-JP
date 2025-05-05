---
title: 品質向上パッチツール
description: 問題を診断し、解決策を見つけて、使用可能なパッチの既存のリストにあるパッチを適用する際に、クオリティパッチツールを使用する方法を説明します。
feature: Cloud, Configuration, Logs, System, Tools and External Services
topic: Architecture, Commerce, Development
role: Admin, Architect, User
level: Beginner, Intermediate
doc-type: Technical Video
duration: 771
last-substantial-update: 2024-07-17T00:00:00Z
jira: KT-15836
exl-id: 16710f27-1232-4c6a-aac3-9838308d1267
source-git-commit: e306b2cd26506f6a7ef37c2d416be7172dc3c0d2
workflow-type: tm+mt
source-wordcount: '542'
ht-degree: 0%

---

# クオリティパッチツール

問題を診断し、解決策を見つけて、使用可能なパッチの既存のリストにあるパッチを適用する際に、クオリティパッチツールを使用する方法を説明します。

## 学習内容

問題をトリアージし、いくつかの基本的な手法を使用して修正を適用する品質パッチを見つける方法を説明します。

## オーディエンス

* 問題を見つけて、このツールを活用して既知の問題に GIT パッチを適用する方法を学ぶ開発者

## ビデオコンテンツ

品質向上パッチツールは、Adobe CommerceおよびMagento Open Source用のコマンドラインユーティリティです。 次の操作を実行できます。

* 最新の品質パッチに関する一般情報を表示します。
* インストール環境に品質向上パッチを適用します。
* 必要に応じて、適用したパッチを元に戻す

これらのパッチは、安定性とパフォーマンスを向上させるため、Adobe開発者やMagento Open Sourceコミュニティによって開発されています。 今後のアップグレードが複雑になる可能性があるので、多数のパッチを適用する場合はお勧めしません。

>[!VIDEO](https://video.tv.adobe.com/v/3431436?learn=on)

## クオリティパッチツールを使用する理由

Adobe CommerceまたはMagento Open Source用の品質向上パッチツールは、次のような場合に使用できます。

安定性とパフォーマンスの向上：品質パッチは、問題への対応、セキュリティの向上、インストールの最適化を実現します。
常に最新の状態を維持：パッチを適用すると、システムを最新の状態に保ち、保護できます。
変更を元に戻す：パッチが原因で予期しない問題が発生した場合は、ツールを使用してパッチを元に戻すことができます。 今後のアップグレードが複雑になるのを避けるため、パッチの適用数が限られている場合に最適です。  

## Quality patch tool の使用に関する制限事項または懸念

品質向上パッチツールにはメリットがありますが、次の点に留意する必要があります。

* 互換性：パッチに、ご使用のAdobe CommerceまたはMagento Open Sourceのバージョンとの互換性があることを確認してください。
* テスト：パッチを実稼動環境に適用する前に、常にステージング環境でテストします。 予期しない問題が発生する場合があります。
* パッチの依存関係：一部のパッチは他のパッチに依存する場合があります。 前提条件に注意してください。
* カスタマイズ：カスタムコードを変更した場合、パッチが競合する可能性があります。 変更内容を慎重に確認します。
* バックアップ：データの損失を防ぐために、パッチを適用する前にインストールをバックアップします。

品質向上パッチツールは、限られた数のパッチを適用する場合には便利ですが、大量のパッチを処理する場合には推奨されません。 適用するパッチが多すぎると、今後のアップグレードやメンテナンスが複雑になる可能性があります。 適用するパッチが多数ある場合は、別の方法を検討するか、Magento担当者に相談してください。 

## 概要

Quality Patches Tool を使用すると、パッチを適用することで e コマースプラットフォームの安定性とセキュリティを強化できます。 これらのパッチは、問題への対応、パフォーマンスの向上、システムの最適化を実現します。 インストールを最新の状態に保つことで、脆弱性に対する保護が確保されます。

パッチを適用する前に、ステージング環境でテストすることが重要です。 Adobe CommerceまたはMagento Open Sourceの特定のバージョンとの互換性を確保します。 一部のパッチには依存関係がある場合があるので、前提条件を慎重に確認してください。

 データの損失を防ぐために、パッチを適用する前にインストールをバックアップします。 カスタムコードを変更した場合は、パッチが競合する可能性があることに注意してください。 ベストプラクティスに従い、各パッチの影響を監視します。

## 関連記事およびビデオ

* [Quality Patch Tools 検索 ](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=ja)
* [ リリースノート ](https://experienceleague.adobe.com/ja/docs/commerce-operations/tools/quality-patches-tool/release-notes)
* [ パッチの GitHub](https://github.com/magento/quality-patches/blob/master/patches/os/)
* [ クオリティパッチツールの利用 ](https://experienceleague.adobe.com/ja/docs/commerce-operations/tools/quality-patches-tool/usage)
* [QPT のテクニカルビデオ ](https://experienceleague.adobe.com/ja/docs/commerce-learn/tutorials/tools/quality-patch-tool)

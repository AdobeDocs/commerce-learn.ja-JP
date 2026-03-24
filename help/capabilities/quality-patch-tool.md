---
title: 品質パッチツール
description: 問題を診断し、解決策を見つけ、利用可能なパッチの既存のリストにあるパッチを適用する際に、品質パッチツールを使用する方法を説明します。
feature: Cloud, Configuration, Logs, System, Tools and External Services
topic: Architecture, Commerce, Development
role: Admin, Developer, User
level: Beginner, Intermediate
doc-type: Technical Video
duration: 903
last-substantial-update: 2024-07-17T00:00:00Z
jira: KT-15836
exl-id: 16710f27-1232-4c6a-aac3-9838308d1267
source-git-commit: b859664f02cf6eac99a551e5f58dff34ca55e37a
workflow-type: tm+mt
source-wordcount: '542'
ht-degree: 0%

---

# 品質パッチツール

問題を診断し、解決策を見つけ、利用可能なパッチの既存のリストにあるパッチを適用する際に、品質パッチツールを使用する方法を説明します。

## 学習すること

問題をトリアージする方法を学び、いくつかの基本的なテクニックを使用して品質のパッチを見つけて修正を適用します。

## オーディエンス

* 問題を見つける方法を学び、既知の問題に対するGIT パッチを適用するためにこのツールを活用する開発者

## ビデオコンテンツ

品質向上パッチツールは、Adobe CommerceおよびMagento Open Sourceのコマンドラインユーティリティです。 Adobe Marketo Engageを利用すれば、次のことが可能になります。

* 最新の品質パッチに関する一般的な情報を表示します。
* インストールに品質パッチを適用します。
* 必要に応じて、適用されたパッチを元に戻す

これらのパッチは、安定性とパフォーマンスを向上させるために、Adobe DevelopersとMagento Open Source コミュニティから開発されます。 今後のアップグレードが複雑になる可能性があるため、多数のパッチを適用するのはお勧めしません。

>[!VIDEO](https://video.tv.adobe.com/v/3431436?learn=on)

## 品質パッチツールを使用する理由

Adobe CommerceまたはMagento Open Sourceの品質パッチツールを使用することをお勧めします。

安定性とパフォーマンスの強化：品質パッチは、問題に対処し、セキュリティを向上させ、インストールを最適化します。
最新の状態を維持：パッチを適用すると、システムが最新で保護されていることを確認できます。
変更を元に戻す：パッチで予期しない問題が発生した場合は、ツールを使用してパッチを元に戻すことができます。 今後のアップグレードを複雑にしないようにするために、限られた数のパッチを適用するのに最適です。  

## 品質パッチツールの使用に関する制限または懸念事項

品質パッチツールにはメリットがありますが、いくつかの考慮事項があります。

* 互換性：パッチが特定のバージョンのAdobe CommerceまたはMagento Open Sourceと互換性があることを確認します。
* テスト：本番環境に適用する前に、必ずステージング環境でパッチをテストします。 予期せぬ問題が発生する可能性があります。
* パッチの依存関係：一部のパッチは、他のパッチに依存する場合があります。 前提条件を把握する：
* カスタマイズ：カスタムコードを変更した場合、パッチが競合する可能性があります。 変更を注意深く確認します。
* バックアップ：パッチを適用する前にインストールをバックアップして、データの損失を防ぎます。

品質パッチツールは、限られた数のパッチを適用する場合には便利ですが、大量のパッチを処理する場合は推奨されません。 パッチを適用しすぎると、今後のアップグレードやメンテナンスが複雑になる可能性があります。 多数のパッチを適用する場合は、別のアプローチを検討するか、Magentoの専門家に相談してください。 

## 概要

品質パッチツールを使用すると、e コマースプラットフォームは、パッチを適用することで安定性とセキュリティを強化できます。 これらのパッチは、問題に対処し、パフォーマンスを向上させ、システムを最適化します。 インストールを最新の状態に保つことで、脆弱性に対する保護を確実に行うことができます。

パッチを適用する前に、ステージング環境でパッチをテストすることが重要です。 お使いのAdobe CommerceまたはMagento Open Sourceの特定のバージョンとの互換性を確認します。 一部のパッチには依存関係がある場合があるので、前提条件を慎重に確認してください。

 データの損失を防ぐために、パッチを適用する前に、インストールをバックアップします。 カスタムコードを変更した場合、パッチが競合する可能性があることに注意してください。 ベストプラクティスに従い、各パッチの影響を監視します。

## 関連する記事とビデオ

* [品質パッチツール検索](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html)
* [ リリースノート ](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/release-notes)
* パッチ用[Github](https://github.com/magento/quality-patches/blob/master/patches/os/)
* [品質パッチツールの使用状況](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/usage)
* [QPTのテクニカル ビデオ ](https://experienceleague.adobe.com/en/docs/commerce-learn/tutorials/tools/quality-patch-tool)

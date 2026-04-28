---
title: 分割支払いPOCの構築：App BuilderとAI ツール
description: 目標、アーキテクチャ、この最初のセッションでカバーされる内容など、App BuilderとCommerce PaaSによる分割払いプルーフについて説明します。
feature: App Builder, Paas, Payments
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Technical Video
duration: 260
jira: KT-20791
source-git-commit: 1e2c7e0e6d0f2d174b88406ce3fb7c787676ecee
workflow-type: tm+mt
source-wordcount: '574'
ht-degree: 0%

---

# 分割支払いPOCの構築：App BuilderとAI ツール

This is the first of a set of tutorials that introduce you to using AI-assisted development to help you build a split payment proof of concept. Adobe App BuilderとAdobe Commerceをクラウド（PaaS）またはオンプレミスで使用する場合は、このセッションの概要から後の実践チュートリアルに移行します。 目標、高度なアーキテクチャ、シリーズの他の部分へのロードマップを期待します。

## ビデオ

>[!VIDEO](https://video.tv.adobe.com/v/3483933?learn=on)

## 重要な詳細


このチュートリアルは、ほとんどのAdobe Commerce チームが現在PHPに深く関わっているところと、Adobeが望んでいるAdobe App BuilderのCommerce外で実行されるイベントドリブン型のサーバーレスビジネスロジックの間のギャップを埋めます。 これは、玩具の例ではなく、実際の実用的な機能を通じて実現します。

### 構築するもの

代金引換とストアクレジットを組み合わせて支払いを行う分割支払いシステム。 注文が完了すると、Commerce管理者を開くことなく、オペレーター（またはERP システム）がスタンドアロンダッシュボードを通じて現金支払いを確認または拒否します。 承認/拒否ワークフロー全体がApp Builderで実行されます。

#### The architectural lesson (core teaching)

このチュートリアルでは、慎重に検討された反復可能な意思決定フレームワークを示します。

* **What must stay in PHP:** anything that runs synchronously in the Commerce request cycle, or that calls Commerce-internal APIs with no clean external surface
* **What moves to App Builder:** everything else — event processing, operator workflow, external integrations, and operator-facing tools

これは「すべてをApp Builderに移す」ことではありません。 書き換えなしで移行を開始する必要があるチームにとって、実用的で正直な出発点です。

### PHP コードが含まれていない理由

AI プロンプトアプローチは、実際には、このユースケースのサンプルコードよりも優れています。なぜなら、このプロンプトは、サンプルコードだけでは伝えられない行動契約とアーキテクチャ上の推論を捉えるからです。 プロンプトを実行して出力を読み取る開発者は、コードが見た目だけでなく、実際の形になっている理由を理解できます。

### 含まれているもの

* App Builderのアプリケーションコードを完成させる（プロジェクト間で一貫性を保つ：直接またはリファレンスとして使用します）
* Commerceモジュール、App Builder Orchestrator、オペレーターダッシュボード、テスト、本番環境へのパスなど、CursorとClaude向けに設計された番号付きのAI プロンプト一式
* 実際のERPを必要とせずに、Commerceのライブサイトに対する承認/拒否フローをテストするためのシミュレーションスクリプト
* あらゆる決定を説明するアーキテクチャドキュメント

### 対象ユーザー

Adobe Commerce オンプレミスまたはCommerce Cloudの開発者で、最初の実際のApp Builder統合を開始する場合。 完全なヘッドレス API ファーストのデプロイメントではなく、特に従来のストアフロントを運用し、既存のPHPへの投資を放棄することなく、実際のビジネスロジックをApp Builderにオフロードする方法を知りたい移行中のチーム向けです。

### 前提条件のコールアウト

Adobe Commerce 2.4.5以降、App Builder プロジェクトを含むAdobe Developer Console組織へのアクセス、およびI/O イベントの有効化。 チェックアウト UIでは、クリーンなLuma テーマが想定されます。カスタムテーマを実行する前に、プロンプトを調整する必要があります。

### まとめ

これは、AI支援による開発に関する初級レベルの議論と考えるべきでしょう。 このチュートリアルは、分割払いについてのチュートリアルだけでなく、Commerce開発ワークフローでAI ツールを使用するためのデモです。

{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

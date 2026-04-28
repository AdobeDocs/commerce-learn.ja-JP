---
title: 分割支払いPOC：チュートリアルクイックリファレンス
description: 分割された支払いPOC ファイルマップについて説明します。どのExperience League ページが各AI プロンプト、推奨されるセクション順序、およびLuma チェックアウトのオーサーノートと一致するかを確認します。
feature: App Builder, Extensibility, Paas, REST, Eventing
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 398
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 1e2c7e0e6d0f2d174b88406ce3fb7c787676ecee
workflow-type: tm+mt
source-wordcount: '1455'
ht-degree: 0%

---

# 分割支払いPOC：チュートリアルクイックリファレンス

このページでは、分割支払い概念実証チュートリアルシリーズの構成方法について説明します。 番号付きの各ソースプロンプトファイルを公開済みのExperience League ページにマッピングし、読者に対するセクションの推奨順序をリスト化し、作成者と管理者に対するメモを収集します。


## ファイル単位の参照

### [分割支払いPOCの作成：App BuilderとAI ツール &#x200B;](create-a-split-payment-poc-app-builder-and-ai-tools.md)

**Source ラベル：** `00_TUTORIAL_OVERVIEW.md` （概念的な概要。公開されたシリーズは、このページで開きます）

**目的：** チュートリアルの概要と方向。

**なぜそれが必要なのか：**&#x200B;開発者に「方法」の前に「理由」を与えます。 構築するもの、オーディエンスは誰か、重要なことは、サイトがLumaのクリーンなインストールと異なる場合にカスタマイズしなければならないことを説明します。 また、シリーズの残りの部分の目次としても機能します。

**チュートリアルの使用：** セクションを開いています。 技術的なステップに進む前に、コンテクストを設定することで、


### [分割支払いPOC: アーキテクチャとデザインの決定](split-payment-poc-architecture-and-decisions.md)


**目的：** PoCにおけるアーキテクチャに関するあらゆる決定を詳細に説明します。

**なぜ必要なのか：**&#x200B;このPoCを操作する際の最も一般的な混乱のポイントは、*なぜ*&#x200B;何かがPHPとApp Builderの比較で発生するのかを理解することです。 このコンテキストがなければ、開発者は（店舗のクレジットアプリケーションなど）移動できないものを移動しようとし、失敗します。 このドキュメントでは、明確な根拠にもとづいて失敗を未然に防ぎます。

**主なトピック：**

* &quot;PHPに何を残すべきか&quot;ルールとその理由
* しきい値の二重強制パターン
* ストアクレジットアプリケーションが非同期にならない理由
* プラグインで処理される5つのCommerce エッジケース
* I/O Event → App Builderのチェックアウト→見積もり→注文→拡張機能の属性データフロー

**チュートリアルの使用：** 「アーキテクチャ」または「仕組み」セクション。 経験豊富なCommerce開発者はスキップできますが、App Builderを初めて利用する場合は必須です。


### [分割支払いPOC：前提条件と環境の設定](split-payment-poc-prerequisites-and-setup.md)


**目的：** コードを記述したり、プロンプトを実行したりする前に、フライト前のチェックリストを完了します。

**必要な理由：** チュートリアルで失敗の割合が最も高いのは、設定フェーズです。 このドキュメントでは、Commerce管理者が正しく設定されていることを確認します（正確なCOD タイトル、ストアクレジットが有効、テストのお客様がバランスを取り、統合が作成されています）。App Builder プロジェクトにはI/O イベントとCommerce イベントプロバイダーが接続され、ローカル環境には適切なバージョンがあります。

**強調アイテム：**

* 代金引換のタイトルは正確に`Cash`である必要があります（クリティカル - JavaScriptは文字列のマッチングを行います）
* OAuth資格情報を機能させるには、Commerce統合を&#x200B;*アクティブ化* （保存されただけでなく）する必要があります
* `entity_id`対`increment_id`は、ここで説明して、全体の混乱を防ぎます

**チュートリアルの使用：** 「前提条件」または「入門」セクション。 読むだけでなく、インタラクティブに完了する必要があります。


### [分割支払いPOC：環境変数リファレンス &#x200B;](split-payment-poc-env-reference.md)


**目的：**&#x200B;すべての3つのコンポーネントのすべての環境変数を1か所に集めます。

**必要な理由：**&#x200B;同じ4つのOAuth資格情報が、変数名が異なる3つの異なるファイル（`COMMERCE_*`対`COMMERCE_INTEGRATION_*`）に表示されます。 1つの参照を持つことで、1つの資格情報セットにタイプミスがある場合の「なぜこれが機能しないか」のデバッグを防ぐことができます。

対象となる&#x200B;**コンポーネント：**

* `split-payment-orchestrator/.env`
* `commerce-checkout-starter-kit/.env` （IMS + OAuth）
* `commerce-backend-ui-1/.env.simulation`

**チュートリアル使用：**&#x200B;参照サイドバーまたは「設定」セクション。 ビルドプロンプトのコンパニオンとしても使用されます。


### [分割支払いPOC: Commerce モジュール AI プロンプト &#x200B;](split-payment-poc-commerce-module-prompt.md)


**目的：** `Client_SplitPayment` PHP モジュール全体を生成するための、自己完結型のAI プロンプトを完成させます。

**なぜ必要なのか：**&#x200B;これは、PHP側の主要なAI ビルドアーティファクトです。 PHPの経験がない開発者が（Claudeを使って） Cursorに渡して動作するモジュールを取得できるように書かれています。 すべてのファイルが指定され、すべての行動コントラクトが定義され、重要な実装メモが含まれ、その後モジュールを有効にするコマンドが提供されます。

**適用範囲：**

* 完全なファイル構造（30以上のファイル）
* データベーススキーマ、拡張属性、REST エンドポイント、I/O イベント設定
* 完全な動作スペックを持つすべてのPHP クラス （署名だけでなく）
* KnockoutJS チェックアウトコンポーネントの仕様
* それぞれが存在する理由を説明した5つのエッジケースプラグイン
* Luma以外のテーマのカスタムコールアウト

**チュートリアルの使用：** 「Commerce モジュールのビルド」セクション。 プロンプト自体は成果物です。開発者はそれをAI ツールにコピーして実行します。


### [分割支払いPOC: App Builder Orchestrator AI プロンプト &#x200B;](split-payment-poc-app-builder-orchestrator-prompt.md)


**目的：** `split-payment-orchestrator` App Builder アプリケーションを生成するための、完全な自己完結型AI プロンプトを実行します。

**必要な理由：** 4つのApp Builder アクション （`payment-orchestrator`、`payment-accept`、`payment-decline`、`demo-dashboard`）は、「PHPから移行したもの」のストーリーの中心です。 このプロンプトは、完全な動作の仕様、正しい`app.config.yaml`構造、およびイベント登録設定を使用して、これらすべてを生成します。

**適用範囲：**

* 4つのアクションとI/O イベント登録をすべて含む`app.config.yaml`
* `commerce-client.js` OAuth 1.0a共有クライアント
* 完全なロジック仕様を備えた4つのアクションすべて
* `store-credit.js`非推奨のスタブ （*なぜ*&#x200B;が使用されていないのか説明します。これは教育学的に重要です）
* HTMLのレンダリング、注文フィルタリング、セキュリティ機能を備えたデモダッシュボード
* すべての変数を含む`.env.example`

**チュートリアルの使用：** 「App Builder アプリケーションのビルド」セクション。 Commerce モジュールプロンプトのコンパニオン。


### [分割支払いPOC: Experience Cloud UI拡張機能AI プロンプト &#x200B;](split-payment-poc-experience-cloud-ui-prompt.md)


**目的：** AI プロンプトを使用して、オプションのExperience Cloud Admin UI SDK拡張機能を生成します。

**必要な理由：** オーケストレータープロンプトのデモダッシュボードは、意図的に概念実証であり、実稼動ではありません。 この節では、次の手順を開発者に示します。分割された支払いダッシュボードを管理者UI SDKを使用してCommerce管理シェルに埋め込みます。 最初のプロンプトにはまったく欠けていました。

**適用範囲：**

* Admin UI SDK拡張機能の`ext.config.yaml`
* 注文ダッシュボードと注文詳細のReact コンポーネント
* リストにIMS認証を使用するバックエンドアクションと、承認/拒否にOAuthを使用するバックエンドアクション
* シミュレーションスクリプト（テストにも使用）

**チュートリアルの使用：** オプション「さらに進む」または「実稼動パス」セクション。 チュートリアルがPoCのみに焦点を当てている場合は、スキップできます。


### [分割支払いPOC：テストと検証ガイド &#x200B;](split-payment-poc-testing-and-verification.md)


**目的：** ステップバイステップのテストガイドで、すべてのコンポーネントを正しい検証順序で説明します。

**なぜ必要なのか：** コンポーネントの作成は作業の半分です。 開発者は、最も低いレベル（データベース列、REST エンドポイント）から始まり、エンドツーエンドのフロー全体に構築される明確な検証パスを必要としています。 元のプロンプトには設定チェックリストがありましたが、明示的なテストフローはありませんでした。

**適用範囲：**

* Commerce モジュールのインストールの検証
* 管理者設定の検証
* REST エンドポイントスモークテスト（curl コマンド）
* チェックアウト UIのチュートリアル（検証ケースを含む）
* しきい値ガードテスト
* フルオーダーのプレースメントテスト
* 承認と辞退のシミュレーションスクリプトの使用
* デモダッシュボードの使用状況
* App Builder アクションログの検査
* 特定の修正を含む10の一般的な障害モード

**チュートリアル使用：** 「テスト」または「検証」セクション。 トラブルシューティングの参考資料としても役立ちます。


### [分割支払POC：概念実証の次の手順](split-payment-poc-next-steps.md)


**PoCを実稼動可能なパターンに進化させるための目的：** ロードマップ。

**なぜそれが必要なのか：** PoC チュートリアルでは、開発者に「今すぐ何をするか？」というリスクが生じます。 フィーリング： このドキュメントでは、デモダッシュボードをExperience Cloud拡張機能に置き換える、実際のERPに接続する、API Meshを追加する、App Builder ワークフローを拡張する、実稼動環境デプロイメントのチェックリストなど、デモから実稼動環境への自然な進行をマッピングします。

**主要コンテンツ：**

* アーキテクチャ進化図（PoC→フェーズ 2→フェーズ 3）
* ERPとの連携パターン（変化、変わらない部分）
* API メッシュブローカーパターン
* 実稼動デプロイメントのチェックリスト
* 主要な参照リンク

**チュートリアルの使用：** 「次の手順」セクションを閉じます。 実際のプロジェクトをスコーピングするための会話のスターターとしても便利です。


## 推奨チュートリアルセクション

これらのファイルに基づいて、読者の一般的な構造は次のとおりです。

| チュートリアルセクション | Experience League page |
| --- | --- |
| 概要と学習目標 | [分割支払いPOCの作成：App BuilderとAI ツール &#x200B;](create-a-split-payment-poc-app-builder-and-ai-tools.md) |
| エンドツーエンドのデモ（動画） | [分割支払いPOCの作成：App Builderの完全デモ &#x200B;](create-a-split-payment-poc-app-builder-and-ai-tools-full-demo.md) |
| 建築：どこに住むか | [分割支払いPOC: アーキテクチャとデザインの決定](split-payment-poc-architecture-and-decisions.md) |
| 前提条件と設定 | [分割支払いPOC：前提条件と環境の設定](split-payment-poc-prerequisites-and-setup.md) |
| 環境変数 | [分割支払いPOC：環境変数リファレンス &#x200B;](split-payment-poc-env-reference.md) |
| 手順1:Commerce モジュールの構築 | [分割支払いPOC: Commerce モジュール AI プロンプト &#x200B;](split-payment-poc-commerce-module-prompt.md) |
| 手順2:App Builder オーケストレーターの構築 | [分割支払いPOC: App Builder Orchestrator AI プロンプト &#x200B;](split-payment-poc-app-builder-orchestrator-prompt.md) |
| ステップ 3：エンドツーエンドのフローをテストする | [分割支払いPOC：テストと検証ガイド &#x200B;](split-payment-poc-testing-and-verification.md) |
| 手順4 （オプション）：管理者UI拡張機能 | [分割支払いPOC: Experience Cloud UI拡張機能AI プロンプト &#x200B;](split-payment-poc-experience-cloud-ui-prompt.md) |
| 次のステップと本番パス | [分割支払POC：概念実証の次の手順](split-payment-poc-next-steps.md) |


## チュートリアル作成者のための重要な注意事項

**Luma テーマの仮定：**&#x200B;すべてのビルドプロンプトで、クリーンなLuma インストール用のコードが生成されます。 チュートリアルでは、上部にこの点を明確に説明する必要があります。カスタムチェックアウトを使用する開発者は、`LayoutProcessorPlugin` インジェクションパスと、場合によってはRequireJS設定を適応させる必要があります。 一連の導入および構築プロンプトには、このためのカスタマイズの呼び出しが含まれています。

**PHP モジュール：**&#x200B;このチュートリアルでは、コピー&amp;ペーストのダウンロードとしてPHP コードを明示的に提供しません。 AI プロンプト *は*&#x200B;を生成します。 これは意図的なものです。ボイラープレートをコピー&amp;ペーストするだけでなく、AIを活用してCommerceの拡張機能を開発するパターンを教えています。 ただし、クリーンな環境でプロンプトが表示されたときに生成されたコードは、`app/code/Client/SplitPayment/`の動作するコードと正確に一致する必要があります。

**$100のしきい値：**&#x200B;これは、ハードコードされたPoC値です。 チュートリアルでは、実際のストアでは、コンパイル時定数ではなくCommerce管理者設定を使用してこれを設定する必要があることに注意してください。

**ストアのクレジット依存関係：**&#x200B;作成された分割支払いフローには`Magento_CustomerBalance` （ネイティブ ストアのクレジット モジュール）が必要です。


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

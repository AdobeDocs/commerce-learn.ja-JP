---
title: 分割払いPOC — App Builderのデモ
description: このLuma デモでは、分割支払い、REST、App Builder I/O、オペレーターが作業を受け入れる/辞退する方法と、カートをブロックできる設定可能な予約注文の合計数について説明します。
feature: App Builder, Paas, Payments
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Technical Video
duration: 861
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '1029'
ht-degree: 0%

---

# 分割払いPOCの作成：App Builderの完全デモ

これは、Adobe CommerceとAdobe App Builder上に構築された分割払い概念実証のエンドツーエンドのチュートリアルです。 このデモでは、既にAI ツールとプロンプトを使用して、処理中のCommerce拡張機能とApp Builder アプリを生成していることを前提としています。このビデオでは、そのコードが結合され、ネイティブのLuma テーマを使用してAdobe Commerce Cloud web サイトにデプロイされ、App Builder プロジェクトが公開された後に何が起こるかを示しています。

買い物客は一部現金と一部&#x200B;**[!UICONTROL Store Credit]**&#x200B;で支払います。 Commerceは、同期チェックアウトとストアフロントに必要なAPIを所有し、App Builderはオーケストレーション、オペレーターワークフロー、I/O イベントコンシューマーを処理します。 参照実装では、多くのマーチャントにとって共通のパスであるEdge Delivery Services ストアフロントではなく、Commerce（PaaS）プロジェクトとLuma ネイティブチェックアウトを使用します。 別のトポロジで&#x200B;**Adobe Commerce as a Cloud Service**&#x200B;を使用する場合、App Builder コードは引き続き似ていますが、ストアフロントと処理中の作業は異なります。 オンプレミス、セルフホスト、Luma上のクラウド内のCommerceの場合、このビデオでは、新しい機能に対するインプロセスコードとApp Builderの実用的な分割を示します。

## ビデオ

>[!VIDEO](https://video.tv.adobe.com/v/3484100?captions=jpn&learn=on)

## この動画は誰のためのものでしょうか？

* Commerceの拡張性を計画するテクニカルアーキテクト
* チェックアウトと統合を実装する開発者
* PHPとApp Builderの現実的な分離が必要な実装チーム

## ストアフロントでシナリオを設定する

デモ アカウントには&#x200B;**[!UICONTROL Store Credit]**&#x200B;があり、チェックアウト時にその残高が表示されます。 注文総額が約80 ドルになるまで商品を追加します。 このサイズでは、計算で争うことなく、カード辞退と同様に、成功した受け入れパスと辞退パスの両方を表示する余地が得られます。

セッションはストア クレジットを公開する必要があるため、**[!UICONTROL Split payment]**&#x200B;はサインインした顧客にのみ提供されます。 ゲストチェックアウトでは、分割オプションは表示されません。

1. **[!UICONTROL Payment]** ステップで、現金方法を選択します（デモでは、**[!UICONTROL Cash on delivery]**&#x200B;の名前が&#x200B;**現金**&#x200B;に変更されます）。
1. 支払い方法の下に分割された支払い領域が表示されます。 キャッシュフィールドは注文合計に事前入力され、コンポーネントはセッションから&#x200B;**[!UICONTROL Store Credit]**&#x200B;を読み取ります。
1. ～$80 カートの場合、**$50**&#x200B;の現金と&#x200B;**$30**&#x200B;のストアクレジットを設定して、コンポーネントに$50 + $30 = $80が表示されるようにします。
1. 金額が有効な場合は、注文します。

UIは、分割の新しい&#x200B;**[!UICONTROL Commerce]** REST APIの呼び出しをトリガーします。 チェックアウトの同期を維持：Commerceは、ストアクレジットを適用し、注文時に分割明細を保存し、App Builderが後で使用するI/O イベントを発生させます。 成功ページは顧客にすぐに提供されます。

## Commerce管理者での注文の確認

**[!UICONTROL Sales]** > **[!UICONTROL Orders]**&#x200B;で、新しい注文を開きます。 **[!UICONTROL Payment information]**&#x200B;領域に分割が表示されます。例えば、**$50**&#x200B;の現金と&#x200B;**$30**&#x200B;の店舗クレジットなどです。 現金がまだ収集されていない場合、ステータスは&#x200B;**保留中**&#x200B;です。注文の作成時には、既にストアクレジットが取得されています。

**[!UICONTROL Comments]**&#x200B;には、App Builderが追加したテキストを含めることができます。 App Builderの&#x200B;**[!UICONTROL Payment orchestrator action]**&#x200B;はI/O イベントを受信し、分割金額を確認し、認証されたRESTを使用してコメントを追加しました。 **[!UICONTROL Commerce]**&#x200B;は他のREST呼び出しと同様に処理され、ライターを知る必要はありません。

## App Builderのオペレーターダッシュボード（ERPまたはバックオフィス）の利用

シンプルなHTML エクスペリエンスは、**[!UICONTROL App Builder]**&#x200B;個のweb アクションとして実行されます（このビューでは&#x200B;**[!UICONTROL Admin]**&#x200B;個ありません）。 ダッシュボードは&#x200B;**[!UICONTROL Commerce Order]**&#x200B;のREST APIを呼び出し、**保留中**&#x200B;にフィルターを実行して、$50の現金足を見つけることができます。

通常、このパターンをERPや不正行為ツールと統合します。 2つのアクションが示されます。

* **受け入れる** – 現金が受け取られたことを確認します（生産中、多くの場合、現金引き出しまたは店舗システムから自動投稿）。
* **辞退** – 不正行為または失敗した収集ステップをシミュレートします（本番環境で、通常はリスクサービスから自動化されます）。

**注文を承諾して完了**

1. **[!UICONTROL App Builder]** アプリで、**Accept**&#x200B;を使用して現金を確認します。
1. アプリは、キャッシュラインを最終処理する新しい&#x200B;**[!UICONTROL Commerce]** エンドポイントを呼び出します。 コア注文フロー（請求書、およびこのビルドの自動出荷）は、ネイティブの&#x200B;**[!UICONTROL Commerce]**&#x200B;動作で実行されます。 このパスには&#x200B;**[!UICONTROL Admin]**&#x200B;に人間が必要ありません。オーケストレーションとエンドポイントはApp Builderにあります。

**[!UICONTROL Admin]**&#x200B;で同じ注文を更新します。現金が受け入れられ、請求書が添付され、製品が発送可能な場合は、デモで完全なライフサイクルを短縮するための出荷が完了すると、ステータスが&#x200B;**完了**&#x200B;に移動します。

**辞退とキャンセル**

1. まだ辞退シナリオにある事前構築済みの注文を開きます。
1. **[!UICONTROL App Builder]** アプリで、**辞退**&#x200B;を使用して、不正行為またはコレクションの失敗をシミュレートします。
1. **[!UICONTROL Admin]**&#x200B;では、注文は&#x200B;**キャンセル済み**&#x200B;です。 履歴には現金ステータスが表示され、コメントには結果が説明され、買い物客は最終的な販売であるかのようにストアクレジットラインに請求されず、ストアクレジットの請求書はキャンセルで無効になります。 また、生産システムは顧客に通知し、在庫やサービスの保留を解除します。

## 注文合計しきい値（注文が作成される前の検証）

App Builderまたは&#x200B;**[!UICONTROL Commerce]**&#x200B;設定は、検証ルールをサポートできます。このビルドでは、**$100**&#x200B;を超える注文を買い物かごの値でブロックします（変更可能なデモの上限）。 値チェックは、最も一般的なビジネス ルールではありません。在庫チェックや可用性チェックはより一般的ですが、注文が作成される前に&#x200B;**[!UICONTROL Commerce]**&#x200B;の&#x200B;**[!UICONTROL Payment]**&#x200B;で検証を実行できることを示していますが、App Builderは引き続きフォローアップの自動処理を行います。

1. 合計が&#x200B;**$100**&#x200B;を超えるまで商品を追加します。
1. 分割&#x200B;**[!UICONTROL UI]**&#x200B;は引き続き表示されます。超過制限結果は&#x200B;**注文**&#x200B;にのみ表示されます。
1. 買い物客に、「**支払いを処理できませんでした」などの一般的なエラーが表示されます。 もう一度試すか、サポートにお問い合わせください。**
1. **[!UICONTROL cart]**&#x200B;は変更されていないため、お客様は合計を減らしたり、別の方法を選択したり、サポートに問い合わせたりできます。

リスクスコア、地域ルール、または類似の項目は、ここでプラグインできます。**[!UICONTROL Commerce]**&#x200B;は注文をブロックし、**[!UICONTROL App Builder]**&#x200B;は通知を所有し、事後処理を実行できます。

## オンプレミスのCommerce、クラウドのCommerce、および&#x200B;**Adobe Commerce as a Cloud Service**

このチュートリアルは、管理するインフラストラクチャの&#x200B;**[!UICONTROL Adobe Commerce]**&#x200B;または従来のストアフロントを使用した&#x200B;**[!UICONTROL Commerce in the cloud]** （PaaS）と一致します。 フロントエンドが異なり、この形状のインプロセスモジュールがない&#x200B;**[!UICONTROL Adobe Commerce as a Cloud service]** （SaaS）の場合、App Builderの部分はほぼ同じですが、ストアフロントと拡張機能の作業は異なります。 いずれの場合も、同じ原則が適用されます。**[!UICONTROL Commerce]**&#x200B;様に&#x200B;**[!UICONTROL Commerce]** リクエスト内で必要な処理を実行させ、残りのエクスペリエンスに&#x200B;**[!UICONTROL App Builder]**&#x200B;を使用させます。

{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

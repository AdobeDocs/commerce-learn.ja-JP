---
title: コンポーザブルカタログデータモデルのカタログビューについて説明します
description: Adobeのコンポーザブルカタログデータモデル（CCDM）のカタログビューで、異なる商品、価格、ルールを使用して、1つのベースカタログを各ストアフロントにマッピングする方法をご確認ください。
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 297
last-substantial-update: 2026-05-15T00:00:00Z
jira: KT-21132
source-git-commit: 96a1356a399fa5cdca9d9befd7c14ebad1b0162f
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 0%

---

# Adobe コンポーザブルカタログデータモデルのカタログビュー

カタログビューとは、単一のコンポーザブルカタログとは異なり、各オーディエンスに異なるサービスを提供する方法です。 このチュートリアルでは、**Carvelo Automobiles**&#x200B;のデモシナリオを使用して、カタログビューとは何か、Adobe Commerce Optimizerが買い物客にどのように適用されるのか、一意のビューIDが商品、価格、規則のアンカーとなる理由について説明します。

## この動画は誰のためのものでしょうか？

* Adobe Commerce Optimizerでマルチブランドまたはマルチディーラーカタログをモデル化するCommerceソリューションアーキテクトおよび開発者

## ビデオコンテンツ

* Carvelo Automobilesのシナリオ（ブランド、ディーラー、契約）
* カタログビューとは何か、統合されたベースカタログ上の「レンズ」比喩
* ストアフロントでカタログビューを使用して商品と価格をフィルタリングする方法（例：Celport）
* カタログビューの一意のIDと、信頼できる唯一の情報源のビジネス価値

>[!VIDEO](https://video.tv.adobe.com/v/3491263?captions=jpn&learn=on)

## シナリオ：Carvelo Automobiles

**Carvelo Automobiles**&#x200B;は、Adobe Commerceのデモ、ドキュメント、チュートリアルで頻繁に使用される架空の自動車部品企業です。 Carveloは、3つのディーラーを通じて、**Aurora**、**Bolt**、**Cruz**&#x200B;の3つのブランドの部品を販売しています。

* **アークブリッジ**&#x200B;はWest Coast Inc.に属しています。
* **Kingsbluff**&#x200B;と&#x200B;**Celport**&#x200B;はEast Coast Inc.に属しています。

各ディーラーは、販売を許可されている製品について独自の契約を締結しています。 これにより、設定が非常に複雑になり、約600万のSKUのうち&#x200B;**単一のベースカタログ**&#x200B;から実行できます。 それを可能にする機能は、**カタログビュー**&#x200B;です。

## カタログビューとは？

**カタログビュー**&#x200B;は、特定のビジネスコンテキストに対するカタログの設定済みビューです。 **レンズ**&#x200B;と考えてください。 統合ベースカタログはその中間に位置し、各ブランドのあらゆるSKUが保持されます。 ディーラーは、それぞれ独自のカタログビューを有しています。

例では、

* **Arkbridge** レンズは、Aurora、Bolt、Cruzの3つのブランドすべてを表示できます。
* **Celport** レンズは、BoltとCruz パーツのサブセットのみを表示します。

各カタログビューでは、**価格**&#x200B;も制御できます。 **の基になるカタログ データは変更されません** – そのコンテキストの表示可能な側面（品揃え、価格、ルール）のみが変更されます。

## Commerce Optimizerでのカタログビューの適用方法

買い物客が&#x200B;**Celport** ストアフロントにアクセスすると、Adobe Commerce Optimizerは&#x200B;**Celport カタログビュー**&#x200B;を使用して、適用される商品、価格、規則を正確に判断します。 買い物客は、そのレンズが可能なものだけを見る。

他の製品（Aurora タイヤ、Bolt motors、Cruz バッテリーなど）は引き続きカタログに含まれますが、**Celportのストアフロントでは、カタログビューで許可されない場合**&#x200B;は表示されません。

## カタログビューIDとビジネス値

各カタログビューには&#x200B;**一意のID**&#x200B;があります。 このIDは、ストアフロントとカタログ設定を結びつけるものです。 一度ストアフロントの設定で設定すれば、その後、下流での行動が追います。適切な商品、適切な価格、的確なルールです。

ディーラーやブランドごとに個別のカタログを管理し、それらを同期させる代わりに、**one**&#x200B;構成可能なカタログを管理します。 カタログビューは、**信頼できる唯一の情報源**&#x200B;から&#x200B;**さまざまなストアフロント体験**&#x200B;を構築する方法です。

## 関連コンテンツ

* [[!DNL Adobe Commerce Optimizer] ガイド](https://experienceleague.adobe.com/ja/docs/commerce/optimizer/overview){target="_blank"}
* [マーチャンダイジング APIの概要](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api/#make-your-first-request){target="_blank"}

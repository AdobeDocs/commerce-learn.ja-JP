---
title: エディションバナー
description: 特定のエディションに適用する機能またはページをメモするための再利用されたビジュアル要素
source-git-commit: 180bfd303df77d95c88fa518253ddff6a67c76d4
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 0%

---

# エディションバナー

## EE のみの機能 {#ee-feature}

<table style="border:1px solid red">
<tr><td><img alt="Adobe Commerce機能" src="../assets/adobe-logo.svg" width="20" height="20" /> Adobe Commerceのみの専用機能（<a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html?lang=ja#product-editions"> 詳細情報 </a>）</td></tr>
</table>

## B2B のみの機能 {#b2b-feature}

<table style="border:1px solid green">
<tr><td><img alt="Adobe Commerce機能" src="../assets/b2b.svg" width="20" height="20" /> Adobe Commerceの <a href="https://experienceleague.adobe.com/docs/commerce-admin/b2b/guide-overview.html?lang=ja">B2B でのみ使用できる専用機能 </a></td></tr>
</table>

## 400 件 {#avoid-400-error}

>[!CAUTION]
>
>API 呼び出しを実行する場合は、何らかの searchCriteria が使用されていることを確認します。 ページネーションの使用も検討することをお勧めします。 Adobe Commerceの結果が大きすぎると、Adobe Developer App Builderの処理能力が満たされ、予期しないファイルの終了が発生する可能性があります。 この結果、400 エラーとして不正な形式の応答が返されました。\
> 例えば、Adobe Commerceから現在のすべての商品をリクエストする必要があるとします。 結果の URL は `{{base_url}}rest/V1/products?searchCriteria=all` のようになります。 返されるカタログのサイズによっては、json が大きすぎてApp Builderで使用できない場合があります。 代わりに、ページネーションを使用し、`Response is not valid 'message/http'.` ストを避けるためにリクエストをいくつか作成します

## PaaS と Commerce Cloud のみ {#only-for-on-prem-commerce-cloud}


**この情報は次の場合に適用されます。**

| オンプレミス | Adobe Commerce Cloud | Adobe Commerceas a Cloud Service | 青部Commerce Optimizer |
| --- | --- | --- | --- |
| はい | はい | 不可 | 不可 |

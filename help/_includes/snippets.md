---
title: バナーを編集
description: 特定のエディションに適用される機能やページをメモするために再利用されたビジュアル要素
source-git-commit: 066e031bd98458c8692f1cb3234ff1ecd1b99e6e
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 0%

---

# バナーを編集

## EE のみの機能 {#ee-feature}

<table style="border:1px solid red">
<tr><td><img alt="Adobe Commerce機能" src="../assets/adobe-logo.svg" width="20" height="20" /> Adobe Commerce (<a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html#product-editions">詳細情報</a>)</td></tr>
</table>

## B2B のみの機能 {#b2b-feature}

<table style="border:1px solid green">
<tr><td><img alt="Adobe Commerce機能" src="../assets/b2b.svg" width="20" height="20" /> 専用の機能はでのみ使用できます <a href="https://experienceleague.adobe.com/docs/commerce-admin/b2b/guide-overview.html">Adobe Commerce用 B2B</a></td></tr>
</table>

## 400 問題 {#avoid-400-error}

>[!CAUTION]
>
>API 呼び出しを実行する際は、何らかの searchCriteria が使用されていることを確認します。 ページネーションの使用も検討してください。 Adobe Commerceの結果が大きすぎる場合、Adobe Developer App Builder の処理能力が満たされ、ファイルが予期せず終了する可能性があります。 結果として、応答の形式が正しくなく、400 エラーが発生しました。\
> 例えば、現在の製品をすべてAdobe Commerceにリクエストする必要があるとします。 結果の URL は次のようになります。 `{{base_url}}rest/V1/products?searchCriteria=all`. 返されたカタログのサイズによっては、JSON が大きすぎて App Builder で使用できない場合があります。 代わりにページネーションを使用し、いくつかのリクエストをおこなって、 `Response is not valid 'message/http'.`

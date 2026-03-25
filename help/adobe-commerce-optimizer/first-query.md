---
title: データのクエリ方法
description: Adobe Commerce Optimizer インスタンスのデータをクエリする方法について説明します。
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 243
last-substantial-update: 2025-08-13T00:00:00Z
jira: KT-18548
exl-id: bad3d926-2952-4bac-b685-adb16f009f8d
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# Query Data Adobe Commerce Optimizer

Adobe Commerce Optimizer インスタンスでGraphQLを使用してデータをクエリする方法について説明します。

## この動画は誰のためのものでしょうか？

* Commerce Solution Architectと開発者

## ビデオコンテンツ

* GraphQLを使用したデータのクエリ
* jqを使用してjsonを読みやすくする

>[!VIDEO](https://video.tv.adobe.com/v/3470800?learn=on)

## コードサンプル

`{{insert-your-graphql-endpoint-url}}`、`{{insert-your-ac-view-id}}`、`{{your-search-query-string}}`などの値を、クエリに必要な値と交換してください。

基本サンプルクエリ

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-View-ID: {{insert-your-ac-view-id}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}'
```

`jq`を使用した基本的なサンプルクエリで、出力を整形します

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-View-ID: {{insert-your-ac-view-id}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}' | jq .
```

## 関連コンテンツ

* [&#x200B; マーチャンダイジング APIの概要](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api/#make-your-first-request){target="_blank"}
* [[!DNL Adobe Commerce Optimizer]  ガイド &#x200B;](https://experienceleague.adobe.com/en/docs/commerce/optimizer/overview){target="_blank"}

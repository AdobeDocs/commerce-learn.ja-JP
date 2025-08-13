---
title: データのクエリ方法
description: Adobe Commerce Optimizer インスタンスのデータをクエリする方法について説明します。
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 182
last-substantial-update: 2025-08-13T00:00:00Z
jira: KT-18548
source-git-commit: c598e46f7119ebdb1575e41c65d6285109fd9af9
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# クエリデータのAdobe Commerce Optimizer

Adobe Commerce Optimizer インスタンスでGraphQLを使用してデータに対してクエリを実行する方法を説明します。

## このビデオの目的は誰ですか。

* Commerce ソリューションアーキテクトと開発者

## ビデオコンテンツ

* GraphQLを使用したデータのクエリ
* jq を使用した json の読み取り容易さ

>[!VIDEO](https://video.tv.adobe.com/v/3470800?learn=on&enablevpops)

## コードサンプル

`{{insert-your-graphql-endpoint-url}}`、`{{insert-your-ac-source-locale}}`、`{{your-search-query-string}}` などの値を、クエリで必要な値と交換してください。

基本的なサンプルクエリ

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-Source-Locale: {{insert-your-ac-source-locale}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}'
```

`jq` を使用して出力をプリティプリントする基本的なサンプルクエリ

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-Source-Locale: {{insert-your-ac-source-locale}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}' | jq .
```

## 関連コンテンツ

* [ マーチャンダイジング API の概要 ](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api/#make-your-first-request){target="_blank"}
* [[!DNL Adobe Commerce Optimizer]  ガイド ](https://experienceleague.adobe.com/ja/docs/commerce/optimizer/overview){target="_blank"}

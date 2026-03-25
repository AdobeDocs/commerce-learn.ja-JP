---
title: 新しいCustomer REST APIについて詳しく見る
description: Adobe Commerce Cloud Serviceで新しい顧客REST APIを使用する方法をご確認ください。 建築家や開発者にとって理想的です。
feature: REST, Customers, Saas
topic: Development, Integrations
role: Developer
level: Beginner
doc-type: Tutorial
duration: 457
last-substantial-update: 2026-01-27T00:00:00Z
jira: KT-20160
exl-id: f40d9b21-1f41-4c76-84a9-161168dbfb1a
source-git-commit: 28257af422ceea62585d4f19ad7c81576c4a3653
workflow-type: tm+mt
source-wordcount: '470'
ht-degree: 0%

---

# Customer REST API

Adobe Commerce as a Cloud Serviceの新しいCustomer REST APIの使用方法を説明します。 このチュートリアルは、API ソリューションを効果的に統合および最適化したいアーキテクトや開発者に最適です。

## この動画は誰のためのものでしょうか？

* Adobe Commerceとの統合の構築を担当するバックエンド開発者
* ヘッドレスコマース導入向けに顧客管理ワークフローを設計するテクニカルアーキテクト

## ビデオコンテンツ

* サーバー間の資格情報を使用してAdobe IMSで認証し、API リクエストのアクセストークンを取得します
* Commerce as a Cloud Serviceに適切なREST API エンドポイントフォーマットを使用する
* 適切なJSON ペイロードを使用して、POSTおよびPUTリクエストを使用してプログラムで顧客アカウントを作成および更新します

>[!VIDEO](https://video.tv.adobe.com/v/3479361?learn=on)

## コードサンプル

開始する前に、[Adobe Developer Console](https://experience.adobe.com)および[Experience Cloud](https://developer.adobe.com/console)から必要なすべての値を収集してください。 これらの値を準備しておくと、設定プロセスがスムーズになります。

>[!NOTE]
>
>正しい組織で作業していることを確認してください。 組織の選択は、Experience CloudとDeveloper Consoleの両方に表示されるインスタンスと環境に影響します。

### インスタンスの詳細 – experience.adobe.com

インスタンスの詳細には、インスタンス ID、GraphQL エンドポイント、資格情報などが含まれます。

### 開発者の詳細 – https://developer.adobe.com/console/

Developer Consoleでは、クライアント ID、クライアントシークレット、アクセストークンなどのAPI資格情報を管理できます。 また、サーバー間やネイティブアプリ認証など、新しい資格情報タイプを作成することもできます。

## 前提条件

| 項目 | 値 | この値はどこにありますか？ |
|--- |--- |--- |
| インスタンス ID | `<instance_id>` | experience.adobe.com |
| REST エンドポイント | `<rest_endpoint>` | experience.adobe.com |
| クライアント ID | `<client_id>` | developer.adobe.com/console |
| クライアント秘密鍵 | `<client_secret>` | developer.adobe.com/console |


## 手順1：アクセストークンの取得（サーバー間認証）

>[!IMPORTANT]
>
> このサンプルに示されている変数は無効です。 プロジェクトの資格情報から&lt;client_id>と&lt;client_secret>を使用します。

```bash
curl -X POST 'https://ims-na1.adobelogin.com/ims/token/v3' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=client_credentials&client_id=<client_id>&client_secret=<client_secret>&scope=openid,AdobeID,email,additional_info.projectedProductContext,profile,commerce.aco.ingestion,commerce.accs,org.read,additional_info.roles'
```

**サンプル応答：**

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIs...",
  "token_type": "bearer",
  "expires_in": 86399
}
```

## 手順2：顧客の作成

>[!IMPORTANT]
>
> このサンプルで指定されたURLは無効です。 REST ベース URLを使用します。 「&lt;rest_endpoint>」をURLと交換します。 この`https://na1-sandbox.api.commerce.adobe.com/AbCYab34cdEfGHiJ27123`と似ています。
>
> このエンドポイントには、URLの一部として/rest/がありません。 それを含めるとエラーになります。

**エンドポイント：** `POST /V1/customers`

```bash
curl -X POST \
  "<rest_endpoint>/V1/customers" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <ACCESS_TOKEN>" \
  -d '{
    "customer": {
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe",
      "store_id": 1,
      "website_id": 1
    },
    "password": "TempPa55word!"
  }'
```

**応答：**

```json
{
  "id": 5,
  "group_id": 1,
  "created_at": "2026-01-23 20:40:15",
  "updated_at": "2026-01-23 20:40:15",
  "created_in": "Default Store View",
  "email": "john.doe@example.com",
  "firstname": "John",
  "lastname": "Doe",
  "store_id": 1,
  "website_id": 1,
  "addresses": [],
  "disable_auto_group_change": 0
}
```

## 手順3：顧客の更新

>[!IMPORTANT]
>
> このサンプルで指定されたURLは無効です。 REST ベース URLを使用します。 「&lt;rest_endpoint>」をURLと交換します。 この`https://na1-sandbox.api.commerce.adobe.com/AbCYab34cdEfGHiJ27123`と似ています。

次の例の数値`5`は、POST `"id": 5,`を使用して以前に作成した顧客のIDです。 リクエストで返されたIDに`5`を必ず変更してください。

**エンドポイント：** `PUT /V1/customers/{customerId}`

```bash
curl -X PUT \
  "<rest_endpoint>/V1/customers/5" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <ACCESS_TOKEN>" \
  -d '{
    "customer": {
      "id": 5,
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe-Updated"
    }
  }'
```

**応答：**

```json
{
  "id": 5,
  "group_id": 1,
  "created_at": "2026-01-23 20:40:15",
  "updated_at": "2026-01-23 20:40:30",
  "created_in": "Default Store View",
  "email": "john.doe@example.com",
  "firstname": "John",
  "lastname": "Doe-Updated",
  "store_id": 1,
  "website_id": 1,
  "addresses": []
}
```

## 完全なスクリプト（オールインワン）

>[!IMPORTANT]
>
> このサンプルに示されている変数は無効です。 プロジェクトの資格情報からクライアント IDとクライアントの秘密鍵を使用します。 REST ベース URLを使用します。 &#39;&lt;rest_endpoint>&#39;をexperience.adobe.comからREST エンドポイント URLと交換します。 この`https://na1-sandbox.api.commerce.adobe.com/AbCDefGHiJ1234567`と似ています。

```bash
#!/bin/bash

# Configuration be sure to update these with your projects unique values
CLIENT_ID="<client_id>"
CLIENT_SECRET="<client_secret>"
REST_ENDPOINT="<rest_endpoint>"

# Step 1: Get Access Token
echo "Getting access token..."
ACCESS_TOKEN=$(curl -s -X POST 'https://ims-na1.adobelogin.com/ims/token/v3' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d "grant_type=client_credentials&client_id=${CLIENT_ID}&client_secret=${CLIENT_SECRET}&scope=openid,AdobeID,email,additional_info.projectedProductContext,profile,commerce.aco.ingestion,commerce.accs,org.read,additional_info.roles" | jq -r '.access_token')

echo "Token obtained: ${ACCESS_TOKEN:0:50}..."

# Step 2: Create Customer
echo ""
echo "Creating customer..."
CREATE_RESPONSE=$(curl -s -X POST \
  "${REST_ENDPOINT}/V1/customers" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ${ACCESS_TOKEN}" \
  -d '{
    "customer": {
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe",
      "store_id": 1,
      "website_id": 1
    },
    "password": "TempPa55word!"
  }')

echo "Create Response:"
echo "$CREATE_RESPONSE" | jq .

# Extract customer ID
CUSTOMER_ID=$(echo "$CREATE_RESPONSE" | jq -r '.id')
echo "Customer ID: $CUSTOMER_ID"

# Step 3: Update Customer
echo ""
echo "Updating customer..."
curl -s -X PUT \
  "${REST_ENDPOINT}/V1/customers/${CUSTOMER_ID}" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ${ACCESS_TOKEN}" \
  -d "{
    \"customer\": {
      \"id\": ${CUSTOMER_ID},
      \"email\": \"john.doe@example.com\",
      \"firstname\": \"john\",
      \"lastname\": \"Doe-Updated\"
    }
  }" | jq .
```

## このチュートリアルに関する注意事項

1. **URL パス**: `https://<server>.api.commerce.adobe.com/<tenant-id>/V1/customers` — **NOT** `https://<host>/rest/<store-view-code>/V1/customers`を使用
1. **認証**：このチュートリアルでは、サーバー間（`client_credentials`付与タイプ）を使用しました
1. **必要な範囲**: `commerce.accs`
1. **トークンの有効期限**: 86400秒（24時間）

## 参照

* [Adobe Commerce as a Cloud Service リリースノート ](https://experienceleague.adobe.com/en/docs/commerce/cloud-service/release-notes)
* [SaaS REST API リファレンス ](https://developer.adobe.com/commerce/webapi/reference/rest/saas/)
* [ ユーザー認証ガイド ](https://developer.adobe.com/commerce/webapi/rest/authentication/user/)

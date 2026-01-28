---
title: 新しいお客様の REST API の調査
description: Adobe Commerce Cloud Service で新しい顧客 REST API を使用する方法について説明します。 アーキテクトや開発者に最適です。
feature: REST, Customers, Saas
topic: Development, Integrations
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 225
last-substantial-update: 2026-01-27T00:00:00Z
jira: KT-20160
source-git-commit: cb3fecf5f8b23425311dc31ed526b3b9bfe07b45
workflow-type: tm+mt
source-wordcount: '470'
ht-degree: 0%

---


# 顧客 REST API

Adobe Commerce as a Cloud Serviceで新しいお客様の REST API を使用する方法を説明します。 このチュートリアルは、API ソリューションを効果的に統合および最適化することを検討しているアーキテクトや開発者に最適です。

## このビデオの目的は誰ですか。

* Adobe Commerceとの統合の構築を担当するバックエンド開発者
* ヘッドレスコマース実装の顧客管理ワークフローを設計するテクニカルアーキテクト

## ビデオコンテンツ

* サーバー間資格情報を使用したAdobe IMSによる認証で API リクエストのアクセストークンを取得
* Commerce as a Cloud Serviceに適した REST API エンドポイント形式を使用する
* 適切な JSON ペイロードを使用した POST リクエストとPUT リクエストを使用して、プログラムでカスタマーアカウントを作成および更新する

>[!VIDEO](https://video.tv.adobe.com/v/3479361/?learn=on&enablevpops)

## コードサンプル

開始する前に、[Experience Cloudと ](https://experience.adobe.com)2}Adobe Developer Console} から必要なすべての値を収集し [ す。 ](https://developer.adobe.com/console)これらの値を準備しておくと、セットアッププロセスをスムーズに実行できます。

>[!NOTE]
>
>正しい組織で作業していることを確認します。 組織の選択は、Experience CloudとDeveloper Consoleの両方に表示されるインスタンスと環境に影響します。

### インスタンスの詳細 – experience.adobe.com

インスタンスの詳細には、インスタンス ID、GraphQL エンドポイント、資格情報などが含まれます。

### 開発者詳細 – https://developer.adobe.com/console/

Developer Consoleで、クライアント ID、クライアントシークレット、アクセストークンなどの API 資格情報を管理します。 また、サーバー間認証やネイティブアプリ認証など、新しい資格情報タイプを作成することもできます。

## 前提条件

| 項目 | 値 | ここで、はこの値です |
|--- |--- |--- |
| インスタンス ID | `<instance_id>` | experience.adobe.com |
| REST エンドポイント | `<rest_endpoint>` | experience.adobe.com |
| クライアント ID | `<client_id>` | developer.adobe.com/console |
| クライアント秘密鍵 | `<client_secret>` | developer.adobe.com/console |


## 手順 1：アクセストークンの取得（サーバー間認証）

>[!IMPORTANT]
>
> このサンプルに示されている変数は有効ではありません。 プロジェクト資格情報の &lt;client_id> と &lt;client_secret> を使用します。

```bash
curl -X POST 'https://ims-na1.adobelogin.com/ims/token/v3' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=client_credentials&client_id=<client_id>&client_secret=<client_secret>&scope=openid,AdobeID,email,additional_info.projectedProductContext,profile,commerce.aco.ingestion,commerce.accs,org.read,additional_info.roles'
```

**応答の例：**

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIs...",
  "token_type": "bearer",
  "expires_in": 86399
}
```

## 手順 2：顧客を作成する

>[!IMPORTANT]
>
> このサンプルで提供された URL は無効です。 REST ベース URL を使用します。 「&lt;rest_endpoint>」を URL と交換します。 次の `https://na1-sandbox.api.commerce.adobe.com/AbCYab34cdEfGHiJ27123` のようになります。
>
> このエンドポイントには、URL の一部として/rest/がありません。 エラーが発生する場合にそれを含めます。

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

## 手順 3：顧客の更新

>[!IMPORTANT]
>
> このサンプルで提供された URL は無効です。 REST ベース URL を使用します。 「&lt;rest_endpoint>」を URL と交換します。 次の `https://na1-sandbox.api.commerce.adobe.com/AbCYab34cdEfGHiJ27123` のようになります。

次の例に示す数値 `5` は、POST `"id": 5,` を使用して以前に作成した顧客の ID です。 必ず `5` をリクエストで返された id に変更してください。

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
> このサンプルに示されている変数は有効ではありません。 プロジェクト資格情報からクライアント ID とクライアントシークレットを使用します。 REST ベース URL を使用します。 experience.adobe.comの REST エンドポイント URL と「&lt;rest_endpoint>」を交換します。 次の `https://na1-sandbox.api.commerce.adobe.com/AbCDefGHiJ1234567` のようになります。

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

## このチュートリアルに関する重要な注意事項

1. **URL パス**:`https://<server>.api.commerce.adobe.com/<tenant-id>/V1/customers` を使用 – **なし** `https://<host>/rest/<store-view-code>/V1/customers`
1. **認証**：このチュートリアルでは、サーバー間（`client_credentials` 付与タイプ）を使用しました
1. **必要な範囲**: `commerce.accs`
1. **トークンの有効期限**:86400 秒（24 時間）

## 参照

* [Adobe Commerce as a Cloud Service リリースノート ](https://experienceleague.adobe.com/en/docs/commerce/cloud-service/release-notes)
* [SaaS REST API リファレンス ](https://developer.adobe.com/commerce/webapi/reference/rest/saas/)
* [ ユーザー認証ガイド ](https://developer.adobe.com/commerce/webapi/rest/authentication/user/)

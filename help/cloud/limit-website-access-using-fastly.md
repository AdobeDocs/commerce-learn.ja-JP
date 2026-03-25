---
title: Fastlyを使用して、web サイト全体へのアクセスを拒否する
description: Fastly Edge ACLとカスタム VCLでAdobe Commerceサイトアクセスを制限する
feature: Cloud, Configuration, Site Management, System
topic: Administration,Commerce,Development, Security
role: Admin, Developer, User
level: Intermediate, Experienced
doc-type: Technical Video
duration: 231
last-substantial-update: 2025-07-11T00:00:00Z
jira: KT-18494
exl-id: 121e7a2f-f9fd-4cd1-b2be-48a12b538008
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Fastlyを使用して、web サイト全体へのアクセスを拒否する

Fastly EdgeのACLとカスタム VCL スニペットを使用して、Adobe Commerce Cloud サイトへのアクセスを制限する方法について説明します。 このステップバイステップのガイドでは、特定のIP アドレスのみを許可し、開発サイトをプライベートに保つことで、ローンチ前の環境を保護するのに役立ちます。

## 学習すること

Fastly Edge ACLとカスタム VCLでAdobe Commerceサイトアクセスを制限| Secure Pre-Launch環境

## この動画は誰のためのものでしょうか？

* DevOps エンジニア
* Adobe Commerce Developer
* サイト信頼性エンジニア

>[!VIDEO](https://video.tv.adobe.com/v/3464779?learn=on)

## コードサンプル

以下は、使用されるVCLの例です

```BASH
if ( !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
```

## 関連ドキュメント

* [悪意のあるIP アドレスの検出](https://experienceleague.adobe.com/en/docs/commerce-learn/tutorials/tools/new-relic/malicious-ip)
* [ リクエストを許可するためのカスタム VCL](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist)
* [要求をブロックするためのカスタム VCL](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-blocking)

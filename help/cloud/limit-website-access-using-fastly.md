---
title: Fastly を使用した web サイト全体へのアクセスの拒否
description: Fastly Edge ACL とカスタム VCL を使用してAdobe Commerce サイトへのアクセスを制限する
feature: Cloud, Configuration, Site Management, System
topic: Administration,Commerce,Development, Security
role: Admin, Developer, User
level: Intermediate, Experienced
doc-type: Technical Video
duration: 200
last-substantial-update: 2025-07-11T00:00:00Z
jira: KT-18494
source-git-commit: 810d1a17e9fe564e8450b091bbeb5574d7d76075
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---


# Fastly を使用した web サイト全体のアクセスの拒否

Fastly Edge ACL とカスタム VCL スニペットを使用して、Adobe Commerce Cloud サイトへのアクセスを制限する方法を説明します。 このステップバイステップガイドは、特定の IP アドレスのみを許可することで起動前環境を保護し、開発サイトをプライベートに保つのに役立ちます。

## 学習内容

Fastly Edge ACL とカスタム VCL を使用してAdobe Commerce サイトへのアクセスを制限する | セキュアなローンチ前環境

## このビデオの目的は誰ですか。

* 開発者エンジニア
* Adobe Commerce開発者
* サイト信頼性エンジニア

>[!VIDEO](https://video.tv.adobe.com/v/3464779/?learn=on&enablevpops)

## コードサンプル

使用する VCL の例を次に示します

```BASH
if ( !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
```

## 関連ドキュメント

* [ 悪意のある IP アドレスの検出 ](https://experienceleague.adobe.com/ja/docs/commerce-learn/tutorials/tools/new-relic/malicious-ip)
* [ リクエストを許可するためのカスタム VCL](https://experienceleague.adobe.com/ja/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist)
* [ ブロックリクエスト用のカスタム VCL](https://experienceleague.adobe.com/ja/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-blocking)
---
title: IP アドレスの検出
description: Adobe Commerce クラウド環境向けのIP アドレスを検出してセキュリティを強化し、サーバー通信を効率化する方法をご紹介します
feature: Cloud, Configuration
topic: Commerce, Development, Integrations
role: Developer
level: Beginner
doc-type: Technical Video
duration: 624
last-substantial-update: 2025-04-07T00:00:00Z
jira: KT-17553
exl-id: beb0a6e1-e6b1-4ec0-976c-77a22a27e8a2
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '1095'
ht-degree: 0%

---

# 異なる環境のIP アドレスを検出する

Adobe Commerce Cloud プロジェクトで異なる環境のIP アドレスを検出する方法について説明します。 Adobe Commerce CLI、sed、xargs、dig、grep、sort -uなどの一連のコマンドを使用すると、開発環境、ステージング環境、実稼動環境のIP アドレスを特定できます。

## この動画は誰のためのものでしょうか？

* 開発者：Adobe Commerce Cloud プロジェクトのIP アドレスを収集する方法を理解したい。
* サードパーティまたはバックエンドシステムへのアクセスを制限する必要があるDevOpsおよびセキュリティチーム

## ビデオコンテンツ {#video-content}

* Adobe Commerce Cloudのあらゆる環境のIP アドレスを明らかにする方法をご紹介します。

>[!VIDEO](https://video.tv.adobe.com/v/3457493?learn=on)

## IP アドレスを取得するコマンド

プレースホルダー情報ではなく、プロジェクト IDと環境名を使用する必要があります。  Adobe Commerce Cloud クラスターのノード数に合わせて`{1..3}`を変更する必要がある場合もありますが、最も一般的なのは3つです。

```bash
magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1 | sed 's/.\.c\.(.)/\1/;s/.$//' | xargs -I% dig +short {1..3}."%" | grep '^\d' | sort -u
```

## Adobe Commerce Cloud CLI

```bash
magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1
```

Magento-Cloud CLI ツールは、開発者やシステム管理者がAdobe Commerce Cloudのプロジェクトや環境を効率的に管理できるように設計されています。 Cloud Consoleの機能を拡張し、ユーザーが日常的なタスクを実行したり、自動化をローカルで実行したりできるようにします。 主な機能には、統合環境の管理、環境のチェックアウトとマージ、変数の一覧表示、リモート環境への接続のSSHの使用などがあります。 このツールは、ローカルワークステーションから直接コマンドを実行できるようにすることで、ワークフローを簡素化し、全体的な開発およびデプロイメントプロセスを強化します。

サンプルコードのこの最初のセクションでは、`magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1`は環境のURLを要求しています。 返される値は、次のようになります：`http://integration-1ajmyuq-mk7xr7zmslfg.us-4.magentosite.cloud/`。 時々この`http://mcprod.russell.dummycachetest.com.c.abcikdxbg789.ent.magento.cloud/`のように見えます。  この最初のコマンドはかなり簡単で、次のコマンドに進む時が来ました。

詳しくは、[Cloud CLIの概要](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/cloud-cli/cloud-cli-overview){target="_blank"}を参照してください

## 検索と置換に`sed`を使用しています

```bash
sed 's/.\.c\.(.)/\1/;s/.$//'
```

UNIX®Linux®のコマンド `sed`は、Stream Editorを表します。 入力ストリーム（ファイルまたはパイプラインからの入力）で基本的なテキスト変換を実行するために使用します。 一般的な用途には、テキストの検索、検索、置換、挿入、削除などがあります。 コマンド `sed`は、テキスト行を行ごとに処理し、指定された操作を適用します。これは、テキスト操作とスクリプト作成のための強力なツールです。

前述のように、通常、`magento-cloud` cliから返されるURLには2つのタイプがあります。 中央に`.com.c.c`を含むバリエーションが1つあります。 このバリエーションは、操作が必要なものです。 この構造が検出された場合は、URLの先頭から`.com.c.c`までのすべての要素を削除する必要があります。  残りはURLの最後の部分に過ぎません。 例のURLは`http://mcprod.russell.dummycachetest.com.c.abcikdxbg789.ent.magento.cloud/`のようです。  このパターンが検出された場合、目標は`.c.`以降のすべてのパターンを保持することです。  このコード例では、`sed 's/.\.c\.(.)/\1/'`がこの部分を取得し、元の返された値の残りを無視するために使用されています。 URLの残りの部分が`abcikdxbg789.ent.magento.cloud/`のようになっています。\
`sed`で2つのコマンドが実行されています。 それらはセミコロンで区切られています。 `sed` コマンド `;s/.$//'`の2番目の部分は、末尾のスラッシュが存在する場合は削除し、そのURLを`abcikdxbg789.ent.magento.cloud`のようにクリーンアップすることです。  この時点で、URLはクリーンアップされ、次のコマンドの準備が整いました。

## 掘り出し付きXargs

```bash
xargs -I% dig +short {1..3}."%"
```

UNIX®Linux®の`xargs` コマンドは、標準入力からコマンドラインを作成および実行するために使用されます。 パイプまたはファイルから入力を受け取り、別のコマンドの引数に変換します。 これは、シェルの制限を超える多数の引数を処理する場合に特に便利です。 コマンド `xargs`を使用して、ファイルの移動、コピー、削除などの操作を実行できます。 1回の実行で複数の引数をコマンドに渡すことで、効率的なバッチ処理が可能になります。

`dig` コマンドは、Domain Information Groperの略で、DNS （Domain Name System） サーバーのクエリに使用するネットワーク管理ツールです。 A、AAAA、MX、CNAME レコードなどのDNS レコードに関する情報を取得するのに役立ちます。 コマンド `dig`は、DNSの問題のトラブルシューティング、DNS設定の検証、ドメイン名と関連するIP アドレスに関する詳細情報の収集に一般的に使用されます。 さまざまなオプションやフラグを使用することで、ユーザーは出力をカスタマイズして、特定の詳細や簡潔な要約を表示できます。

`xargs`を`dig`と共に使用すると複雑になりますが、必須です。 目標は、クリーンアップされたURLを取得し。  URLが変数として保存されると、`dig` コマンドに挿入されます。

コマンド `dig`は、DNS情報を収集するために作成されました。 返されるデータ量を減らすには、引数`+short`を使用します。 `dig`を`+short`と組み合わせて使用すると、IP アドレスと文字列が返されることがあります。

コマンドのその部分では、`xargs`はそのURL `abcikdxbg789.ent.magento.cloud`を保存し、次のコマンド `dig`に挿入されます。 反復と組み合わせてURLを保存する手法により、`dig` コマンドでの使用が簡単になります。 私のサンプルコードは目標を達成するための1つの方法であることに留意してください、あなたのニーズと期待を満たすために物事を修正してください。

この時点で、URLの準備ができました。 次に、クラスター内の各サーバーを確認する方法を見てみましょう。 Adobe Commerce Cloudの場合、クラスター内の各サーバーには番号があります。 サーバー識別子は、クリーンアップされ、使用の準備が整ったURLのプレフィックスです。 サーバーのチェックオフを素早く簡単に行うには、`{1..3}`を使用します。 `{1..3}`を使用して、`dig` コマンドを3回実行することを通知します。 以下は、実行をリアルタイムで見た場合の動作を表しています。

```bash
dig +short 1.<projectid>.ent.magento.cloud
dig +short 2.<projectid>.ent.magento.cloud
dig +short 3.<projectid>.ent.magento.cloud
```

より詳しい説明のために、`dig`が使用しているこの例のURLは次のようになります。

```bash
dig +short 1.aabcikdxbg789.ent.magento.cloud
dig +short 2.abcikdxbg789.ent.magento.cloud
dig +short 3.abcikdxbg789.ent.magento.cloud
```

`{1..3}`が`{1..6}`に変更された場合、次のようになります。

```bash
dig +short 1.aabcikdxbg789.ent.magento.cloud
dig +short 2.abcikdxbg789.ent.magento.cloud
dig +short 3.abcikdxbg789.ent.magento.cloud
dig +short 4.aabcikdxbg789.ent.magento.cloud
dig +short 5.abcikdxbg789.ent.magento.cloud
dig +short 6.abcikdxbg789.ent.magento.cloud
```

## コマンド `grep`

`dig`の結果の一部として文字列が返されることがあります。 この目的のために、目標はIP アドレスのみであり、それらは期間を持つ数字で構成されています。 最終的な出力に数値のみを含めるには、コマンドを調整できます。 完了すると、最終的な構文は` grep '^\d'`になります。  `'^\d'`を追加すると、コマンド `grep`は数値のみを保持し、他のものを無視します。

## コマンド `sort`

`sort -u`を使用すると、重複がIPのリストから削除されます。 重複は、開発レベルを調べるときにのみ発生します。

これらの下位レベルの環境はマルチテナントで、その基盤となるサーバーを他の多くのプロジェクトと共有します。 開発環境は単一のサーバーであり、クラスターではありません。 したがって、dig コマンドが各イテレーションをループしている場合、同じIPを何度も返します。 したがって、コマンド `sort -u`を使用すると、重複するすべてのIPが削除され、一意のIP アドレスのみが残ります。



## 関連ドキュメント

* [地域IP アドレス ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/project/regional-ip-addresses){target="_blank"}

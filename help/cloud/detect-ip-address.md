---
title: IP アドレスの検出
description: Adobe Commerce クラウド環境の IP アドレスを検出して、セキュリティを強化しサーバー通信を効率化する方法について説明します
feature: Cloud, Configuration
topic: Commerce, Development, Integrations
role: Developer
level: Beginner
doc-type: Technical Video
duration: 0
last-substantial-update: 2025-04-07T00:00:00Z
jira: KT-17553
exl-id: beb0a6e1-e6b1-4ec0-976c-77a22a27e8a2
source-git-commit: 3acec65129773a8ba94eb52c53d15d7633440717
workflow-type: tm+mt
source-wordcount: '1095'
ht-degree: 0%

---

# 様々な環境の IP アドレスを検出

Adobe Commerce Cloud プロジェクトで様々な環境の IP アドレスを検出する方法について説明します。 Adobe Commerce CLI、sed、xargs、dig、grep、sort -u などの一連のコマンドを使用して、開発環境、ステージング環境、実稼動環境の IP アドレスを識別できます。

## このビデオの目的は誰ですか。

* 開発者：Adobe Commerce Cloud プロジェクトの IP アドレスを収集する方法を理解しようとしています。
* サードパーティまたはバックエンドシステムへのアクセスを制限する必要がある DevOps およびセキュリティチーム

## ビデオコンテンツ {#video-content}

* Adobe Commerce Cloud の任意の環境の IP アドレスを明らかにする方法について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3457493/?learn=on)

## IP アドレスを取得するコマンド

プレースホルダー情報の代わりにプロジェクト ID と環境名を使用する必要があります。  また、Adobe Commerce Cloud クラスター内のノード数に合わせて `{1..3}` を変更する必要が生じる場合もありますが、最も一般的なのは 3 つです。

```bash
magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1 | sed 's/.\.c\.(.)/\1/;s/.$//' | xargs -I% dig +short {1..3}."%" | grep '^\d' | sort -u
```

## Adobe Commerce Cloud CLI

```bash
magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1
```

Magento-cloud CLI ツールは、開発者とシステム管理者が Adobe Commerce Cloud のプロジェクトと環境を効率的に管理するのに役立つように設計されています。 Cloud Console の機能を拡張し、ユーザーが日常的なタスクを実行し、自動化をローカルで実行できるようにします。 主な機能には、統合環境の管理、環境のチェックアウトとマージ、変数の一覧表示、リモート環境への接続に SSH を使用することなどが含まれます。 このツールを使用すると、ローカルのワークステーションから直接コマンドを実行できるようになり、開発およびデプロイメントプロセス全体が強化されるので、ワークフローが簡単になります。

サンプルコードのこの最初のセクション `magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1` は、環境の URL を要求しています。 戻り値は次の `http://integration-1ajmyuq-mk7xr7zmslfg.us-4.magentosite.cloud/` のようになります。 時々この `http://mcprod.russell.dummycachetest.com.c.abcikdxbg789.ent.magento.cloud/` のように見えます。  この最初のコマンドはかなり簡単で、次のコマンドに沿って移動する時が来ました。

詳しくは、[Cloud CLI の概要 ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/cloud-cli/cloud-cli-overview){target="_blank"} を参照してください。

## 検索と置換に `sed` を使用

```bash
sed 's/.\.c\.(.)/\1/;s/.$//'
```

UNIX®Linux® のコマンド `sed` はストリームエディターを表します。 入力ストリーム（ファイルまたはパイプラインからの入力）で基本的なテキスト変換を実行するために使用されます。 一般的な使用方法には、テキストの検索、検索と置換、挿入、削除などがあります。 コマンド `sed` は、テキストを 1 行ずつ処理し、指定された操作を適用するので、テキスト操作とスクリプト作成のための強力なツールになります。

前述のように、通常、`magento-cloud` cli から返される URL には 2 つのタイプがあります。 中間に `.com.c.c` を含むバリエーションが 1 つあります。 このバリアントは、操作する必要があるものです。 この構造が検出された場合は、URL の先頭から `.com.c.c` までのすべてを削除する必要があります。  すると、残っているのは URL の最後の部分だけです。 URL の例は `http://mcprod.russell.dummycachetest.com.c.abcikdxbg789.ent.magento.cloud/` のようになります。  このパターンが検出された場合、目標は検出された後にすべてを保持するこ `.c.` です。  提供されるこのサンプルコードでは、`sed 's/.\.c\.(.)/\1/'` を使用してこの部分を取得し、元の戻り値の残りの部分を無視します。 URL の残りの部分は、`abcikdxbg789.ent.magento.cloud/` のようになります。\
`sed` で実行されるコマンドは 2 つあります。 これらはセミコロンで区切られています。 `sed` コマンド `;s/.$//'` の 2 番目の部分は、末尾のスラッシュが存在する場合は削除し、その URL を `abcikdxbg789.ent.magento.cloud` のようにクリーンアップすることです。  この時点で、URL はクリーンアップされ、次のコマンドの準備が整いました。

## 掘り出した Xargs

```bash
xargs -I% dig +short {1..3}."%"
```

UNIX®Linux® の `xargs` コマンドを使用して、標準入力からコマンドラインを作成して実行します。 パイプまたはファイルから入力を受け取り、別のコマンドの引数に変換します。 シェルの制限を超える多数の引数を処理する場合に特に便利です。 コマンド `xargs` を使用すると、ファイルの移動、コピー、削除などの操作を実行できます。 1 回の実行でコマンドに複数の引数を渡すことで、効率的なバッチ処理が可能になります。

`dig` コマンドは、Domain Information Groper の略で、DNS （ドメイン ネーム システム）サーバーの照会に使用されるネットワーク管理ツールです。 A、AAAA、MX、CNAME レコードなど、DNS レコードに関する情報を取得するのに役立ちます。 コマンド `dig` は、DNS の問題のトラブルシューティング、DNS 設定の検証、ドメイン名とそれに関連する IP アドレスに関する詳細情報の収集に一般的に使用されます。 様々なオプションとフラグを使用して、特定の詳細や簡潔な概要を表示するように出力をカスタマイズできます。

`dig` を使用した `xargs` の使用はそれを複雑にしますが、必要です。 目標は、クリーンアップされた URL を取得して保存することです。  URL が変数として保存されると、`dig` コマンドに挿入されます。

DNS 情報を収集するためにコマンド `dig` が作成されました。 返されるデータの量を減らすには、引数 `+short` を使用します。 `dig` を `+short` と組み合わせて使用すると、IP アドレスと、場合によっては文字列が返されます。

コマンドのこの部分では、`xargs` はその URL `abcikdxbg789.ent.magento.cloud` を保存し、次のコマンド `dig` ードに挿入します。 イテレーションと組み合わせて URL を保存する手法により、`dig` コマンドでの使用が容易になります。 私のサンプルコードは目標を達成するための 1 つの方法であることに注意してください。ニーズや期待に合わせて自由に変更できます。

この時点で、URL の準備が整っています。 次に、クラスター内の各サーバーを確認する方法を見てみましょう。 Adobe Commerce Cloud の場合、クラスター内の各サーバーには番号が付けられます。 サーバー識別子は、クリーンアップされて使用する準備が整った URL のプレフィックスです。 `{1..3}` を使用すると、サーバーを素早く簡単にチェックアウトできます。 `dig` のコマンドを 3 回実行することを通知する `{1..3}` を使用する。 次に、実行をリアルタイムで監視する場合の動作を示します。

```bash
dig +short 1.<projectid>.ent.magento.cloud
dig +short 2.<projectid>.ent.magento.cloud
dig +short 3.<projectid>.ent.magento.cloud
```

例を挙げると、`dig` で使用される URL の例は次のようになります。

```bash
dig +short 1.aabcikdxbg789.ent.magento.cloud
dig +short 2.abcikdxbg789.ent.magento.cloud
dig +short 3.abcikdxbg789.ent.magento.cloud
```

`{1..3}` が `{1..6}` に変更された場合、次のようになります。

```bash
dig +short 1.aabcikdxbg789.ent.magento.cloud
dig +short 2.abcikdxbg789.ent.magento.cloud
dig +short 3.abcikdxbg789.ent.magento.cloud
dig +short 4.aabcikdxbg789.ent.magento.cloud
dig +short 5.abcikdxbg789.ent.magento.cloud
dig +short 6.abcikdxbg789.ent.magento.cloud
```

## コマンド `grep`

`dig` からの結果の一部として文字列が返される場合があります。 この目的のために、目標は IP アドレスのみであり、それらはピリオドを持つ数字で構成されています。 最終的な出力に数値のみが含まれるようにするには、コマンドを調整します。 完了すると、最終的な構文は ` grep '^\d'` になります。  `'^\d'` を追加すると、コマンド `grep` は数値のみを保持し、それ以外は無視します。

## コマンド `sort`

`sort -u` を使用すると、IP のリストから重複が削除されます。 重複は、開発レベルを調べた場合にのみ発生します。

これらの下位レベルの環境はマルチテナントであり、基盤となるサーバーを他の多くのプロジェクトと共有しています。 開発環境は単一のサーバーであり、クラスターではありません。 したがって、dig コマンドが各反復をループしている場合、同じ IP を何度も返します。 そのため、`sort -u` コマンドを使用すると、重複した IP はすべて削除され、一意の IP アドレスのみが残ります。



## 関連ドキュメント

* [ 地域 IP アドレス ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/project/regional-ip-addresses|https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/project/regional-ip-addresses){target="_blank"}

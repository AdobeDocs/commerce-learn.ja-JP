---
title: Adobe Commerce Cloud導入前のチェックリスト
description: Adobe Commerce Cloudの導入前のチェックリストと、稼働前にインテグレーターと連携して作業する方法について説明します。
feature: Cloud
topic: Commerce, Architecture, Development
role: Admin, Developer, User
level: Intermediate
doc-type: Tutorial
duration: 451
last-substantial-update: 2024-04-17T00:00:00Z
jira: KT-15180
exl-id: c6adb2c2-f194-4a3d-9290-e0837ef062ae
source-git-commit: 8266ad03ec3bd9364bb9093c8876a4dc1507e1a2
workflow-type: tm+mt
source-wordcount: '1674'
ht-degree: 0%

---


# 起動前のチェックリスト

このページでは、Adobe Commerce [ サイト起動ドキュメント ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/launch/overview){target="_blank"}の概要を説明します。

このチェックリストは、Adobe Commerce Cloud サイトのローンチを成功させるための計画と実行を支援することを目的としています。 Adobe Commerce Cloudのシステムインテグレータと連携して、すべての設定タスクとチェックリスト項目が完了し、検証されるようにします。 ご不明な点がございましたら、お気軽にカスタマーテクニカルアドバイザーまたはカスタマーサクセスエンジニアまでお問い合わせください。 アカウントに割り当てられたCTA/CSEがない場合は、サポートチケットを作成してサポートを受けることができます。

アカウントにCTA/CSEが割り当てられている場合は、新しいAdobe Commerce Cloud サイトを立ち上げる4週間前までに、アカウントとアカウントマネージャーに連絡し、**の立ち上げ意向**&#x200B;を通知してください。

* 一部のチェックは[!BADGE Blocker]{type=caution tooltip="潜在的な課題"}で強調表示されます。慎重にレビューしないと、公開がブロックされる可能性があります。
* 開発者やシステム統合パートナーと連携して、導入アプローチの整合性を維持します。

>[!IMPORTANT]
> このチェックリストを使用して完了しなかった場合、実稼動開始スケジュールおよび継続的なサイトの安定性に対する悪影響および関連リスクについて、[責任](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"}を受け入れます。

## &#x200B;1. 運用開始前

1. テストと公開に関するドキュメントを確認する[ サイト起動ドキュメント ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/launch/overview){target="_blank"}

   >[!NOTE]
   >包括的な&#x200B;_「運用開始準備計画」_&#x200B;が、パートナーまたはシステムインテグレーターで完全に準備されていることを確認し、必要なすべてのアクション項目を組み込みます。 リリース前のチェックリストでは、Adobeのベストプラクティスを強調していますが、_**では、お客様自身の運用開始準備計画の必要性を置き換えることはできません**_。

2. [!BADGE  ブロッカー]{type=caution tooltip="潜在的な課題"} サポートインサイト （SWAT）の推奨事項と情報を確認します（[ ユーザーガイド ](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/intro){target="_blank"}）
3. エンドユーザーと加盟店が、バックエンド業務を含むUAT （ユーザー受け入れテスト）を完了したことを確認します。
4. システムインテグレーターチームが、ステージングと実稼動に対してエンドツーエンドのUATを実行したことを確認します。 [Experience League ドキュメント ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/test/staging-and-production){target="_blank"}を参照してください。
5. ステージング環境と実稼動環境でのコードのデプロイメントとテストを確認します（[詳細情報](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/test/staging-and-production){target="_blank"}）。
6. 実稼動クラスターが、契約した1日のベースラインに対して永続的にアップサイズされたことを確認します。 詳しくは、割り当てられたCTA/CSEに問い合わせるか、サポートチケットを発行してください。

## &#x200B;2. 現行コンフィギュレーション

1. Adobe Commerceと関連パッケージ/サービスを[最新バージョン ](https://experienceleague.adobe.com/en/docs/commerce-operations/release/notes/overview){target="_blank"}にアップグレードします
2. 現在の設定とサービスをSI/パートナーと確認し、[ ベストプラクティスに従います](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/catalog-management){target="_blank"}。
3. MySQL/Shared-Files [ ディスク使用状況](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/storage/manage-disk-space){target="_blank"}を確認します

## &#x200B;3. Fastly設定

1. [!BADGE  ブロッカー]{type=caution tooltip="潜在的な課題"} キャッシュが機能していることを確認してください（[ フルページキャッシュ ](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/tools/cache-management){target="_blank"}または[GraphQL キャッシュ ](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}）。 [Fastlyの設定ガイド ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/fastly){target="_blank"}を読む。
2. 該当する場合は、PWA/ヘッドレス web サイトでGraphQL クエリに対してGET メソッドを使用します。

   >[!NOTE]
   > HTTP GET操作で送信されたクエリのみがキャッシュできます（該当する場合）。 [POST クエリをキャッシュできません](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}。

3. Fastly Image Optimizationが有効になっていることを確認します（[Fastly Image Optimization](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/fastly-image-optimization){target="_blank"}を参照）
4. 正しいシールドの場所が設定されていることを確認します（[ キャッシュ、バックエンド、オリジンのシールドを設定](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/setup-fastly/fastly-custom-cache-configuration){target="_blank"}）。
5. Web アプリケーションファイアウォール （**WAF**）が機能していることを確認します。 （[ ブロックされたリクエストのトラブルシューティング ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/fastly-waf-service){target="_blank"}がある場合は、制限を参照してください）。
6. 管理者パネルの「無視されたURL パラメーター」 [ リストをFastly ](https://github.com/iancassidyweb/magento2/commit/68fdecfcd26c957382b8d68b64887e0a83298524){target="_blank"}&quot;に更新して、キャッシュのパフォーマンスを向上させます。

   >[!NOTE]
   > _管理者/ストア/設定/システム/フルページキャッシュ/Fastly設定/詳細設定/無視URL パラメーター（グローバル）_&#x200B;の下にあるFastly設定で、キャッシュされたページの検索時にFastlyが無視するパラメーターのコンマ区切りリストを見つけることができます。 このリストを変更した後、VCLを再アップロードします。

## &#x200B;4. DNSとSSL

1. [!BADGE  ブロッカー]{type=caution tooltip="潜在的な課題"}必要なすべてのドメイン名が要求されていることを確認します。 _（追加または変更されたドメインについては、事前にサポートチケットを送信してください）_
2. [!BADGE Blocker]{type=caution tooltip="潜在的な課題"} SSL （TLS）証明書がドメインに適用されました。 詳しくは、[この記事](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/ssl-tls-certificates-for-magento-commerce-cloud-faq){target="_blank"}を参照してください。
3. DNS [TTL （Time to Live） ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/launch/checklist){target="_blank"}の値を可能な限り最小限に更新して、運用開始します。
4. SendGrid SPFとDKIMを有効にします。

   >[!NOTE]
   > 各ドメインのSendGrid CNAME レコードをDNS設定に追加します。 [SendGrid メールサービス ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/project/sendgrid){target="_blank"}を読んで、送信者ドメインなどを変更する方法をご確認ください。

## &#x200B;5. データベース設定

Adobe Commerce Cloudでは、ステージング環境と実稼動環境の両方のデータベースとしてMariaDB Galera クラスターを採用しています。 Galera クラスターは、パフォーマンスと拡張性を強化するのに役立ちます。 Galera クラスターレプリケーションの最適なプラクティスと制約に関するインサイトを得るには、次の記事を参照してください。

* [MySQL設定のベストプラクティス ](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration){target="_blank"}
* Adobe Commerceの管理アラート：[MariaDB アラート ](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/managed-alerts-for-adobe-commerce/managed-alerts-on-magento-commerce-mariadb-alerts){target="_blank"}
* [ データベース設定](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud){target="_blank"}のベストプラクティス
* [ ガレラクラスターのレプリケーションとフロー制御](https://experienceleague.adobe.com/en/docs/commerce-learn/tutorials/extensibility/backend-development/galera-db-slow-replication){target="_blank"} （詳細分析）

1. データベースの高い読み込み時にパフォーマンスを向上させるには、[MySQL スレーブ接続](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration#slave-connections){target="_blank"}をお勧めします。
2. すべてのデータベーステーブルの行形式が、COMPACT[ではなく](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/maintenance/mariadb-upgrade#convert-database-table-storage-format){target="_blank"}DYNAMICに設定されていることを確認します（オンプレミスからクラウドへの移行の場合は特に重要です）。
3. すべてのテーブルのデータベースストレージエンジンを[MyISAMからInnoDB](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud#convert-all-myisam-tables-to-innodb){target="_blank"}に変更します。
4. 1 GBを超えるデータベーステーブルを事前に確認して最適化します。
5. データベーススキーマ情報は最新かつ最新です。 （[このガイド ](https://mariadb.com/docs/server/ha-and-performance/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/engine-independent-table-statistics#collecting-statistics-with-the-analyze-table-statement){target="_blank"}を参照）。

## &#x200B;6. 展開

1. 静的コンテンツ展開（SCD）の理想的な状態を確認して、実稼動環境での展開中のメンテナンス時間を短縮します。 [静的コンテンツ展開（SCD）戦略](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/deploy/static-content){target="_blank"}および[ ストア構成管理](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure-store/store-settings){target="_blank"} ガイドを確認してください。
2. HTML、JavaScript、CSSの最小化設定を確認します。 （これは、PWA/ヘッドレスサイトには適用されません）。
3. 次のクラウド変数の利用が、意図した目的に合致していることを確認します。 （[SCD_MATRIX](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/stage/variables-build#scd_matrix){target="_blank"}、[SCD_ON_DEMAND](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/stage/variables-global#scd_on_demand){target="_blank"}、[SKIP_SCD](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/stage/variables-deploy#skip_scd){target="_blank"}）

## &#x200B;7. テストとトラブルシューティング

1. 送信するトランザクションメールをテストします。 [Adobe Commerce Cloud - SendGrid メール機能](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/project/sendgrid){target="_blank"}の詳細をご覧ください。
2. [!BADGE  ブロッカー]{type=caution tooltip="潜在的な課題"}起動するAdobe関連のブロッカーがないことを確認します。
3. [!BADGE  ブロッカー]{type=caution tooltip="潜在的な課題"}本番稼動インスタンスで負荷および負荷テストを実行し、割り当てられたCTA/CSEと結果を共有します。

   >[!NOTE]
   > [負荷および負荷テストは、アプリケーション内のボトルネックを特定し、パフォーマンスの問題を明らかにする目的](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/test/guidance#:~:text=A%20load%20test%20can%20help,Scan%20Tool%20for%20your%20sites.){target="_blank"}を果たします。 これは、クラスタのサイズに関する期待を管理し、ビジネス要件を効果的に満たすために必要なスケーリング調整を決定する上で重要な役割を果たします。

   >[!IMPORTANT]
   > **_WARNING:_**&#x200B;読み込みテストを準備する際、**はライブトランザクションメールを（ダミーアドレスにも）送信しません**。 テスト中にメールを送信すると、プロジェクトが起動前にSendGrid用に設定されたデフォルトの送信制限（12k）に達する可能性があります。
   > 
   > * メール通信を無効にする方法：
   >   _ストア/設定/詳細/システム/電子メール送信設定_&#x200B;に移動します。

4. [共有責任セキュリティモデル ](https://business.adobe.com/products/commerce.html){target="_blank"}の一部として、実稼動インスタンスに対してセキュリティ侵入テストを実施します。 PCI （ペーメントカード業界）認定を取得するためには、カスタマイズされたサイトで侵入テストを実施する必要があります。

## &#x200B;8. その他の構成

1. インデックス作成を&#x200B;_「スケジュールに合わせて更新_」に切り替えます。ただし、**_customer_grid_**&#x200B;は「保存」に残ります（[ インデックスモード ](https://developer.adobe.com/commerce/php/development/components/indexing/#indexing-modes){target="_blank"}を参照）。
2. 使用中のサードパーティ検索エンジンや拡張機能のドキュメント化。
3. 関連する場合は、[SEO （検索エンジン最適化）設定が正しく設定されていることを確認して、インデクサー/web クローラーがweb サイトをスキャンできるようにします](https://experienceleague.adobe.com/en/docs/commerce-admin/marketing/seo/seo-overview){target="_blank"}。
4. リダイレクトとルートの追加（[ ルートの設定](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/routes/routes-yaml){target="_blank"}を参照）

   >[!NOTE]
   > 統合環境のroutes.yaml ファイルにリダイレクトとルートを追加し、ステージングと実稼動にデプロイする前に、この環境の設定を確認します。

   `routes.yaml` フラグメントの例：

   ```yaml
           "http://{all}/":
               type: upstream
               upstream: "mymagento:http"
   
           "http://{all}/":
               type: upstream
               upstream: "mymagento:http"
   ```

5. 開発中にXdebugが有効になっていることを確認します（[Commerce Cloud版Xdebugの設定](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/test/debug){target="_blank"}を参照）。
6. OPcacheおよびその他の設定がphp.ini ファイルで正確に更新されていることを確認します（[このサンプル ](https://github.com/magento/magento-cloud/blob/master/php.ini#L41){target="_blank"}を参照）。
7. [**Adobe Commerceのステータス ページ**](https://status.adobe.com/cloud/experience_cloud#/){target="_blank"}を購読します。
8. New Relicの「[Adobe Commerceの管理アラート ](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/managed-alerts-for-adobe-commerce/managed-alerts-for-magento-commerce){target="_blank"}」通知チャネルを購読して、指定されたパフォーマンス指標をモニタリングします（[詳細を読む](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/monitor/new-relic/new-relic-service){target="_blank"}）。

## &#x200B;9. セキュリティ

1. Adobe Commerce Security Scanを設定します。

   >[!NOTE]
   > [Adobe Commerce セキュリティ スキャンは、古いソフトウェア バージョン、正しくない構成、およびサイト上の潜在的なマルウェアを検出するのに役立つツールです](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-scan){target="_blank"}。 サインアップし、頻繁に実行するようにスケジュールを設定し、メールが適切なテクニカルセキュリティ担当者に送信されるようにします。
   > 
   > UAT中にこのタスクを完了します。 定期的なスキャン オプションを使用する場合は、必ず低需要時間でスキャンをスケジュールしてください。 Adobe Commerce アカウントにログインした後、アカウントからセキュリティスキャンツールを開きます（アクセスと使用については、[ セキュリティスキャン ](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-scan){target="_blank"}を参照）。

2. Adobe Commerce管理者のデフォルト設定を変更します。
3. 管理者パスワードを変更します（[管理者セキュリティの設定](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-admin){target="_blank"}を参照）。
4. 管理者URLを変更します（[ カスタム管理者URLの使用](https://experienceleague.adobe.com/en/docs/commerce-admin/stores-sales/site-store/store-urls#use-a-custom-admin-url){target="_blank"}を参照）。
5. プロジェクトに存在しないユーザーを削除します（[ ユーザーの作成と管理](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/project/user-access){target="_blank"}を参照）。
6. 管理者パスワードが要件を満たしていることを確認します（[管理者パスワード要件](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-admin){target="_blank"}を参照）。
7. 二段階認証を設定します（[二段階認証（管理者） ](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-admin#two-factor-authentication){target="_blank"}を参照）。

## &#x200B;10. 本番稼動

切り替える時期になったら、次の手順を実行してください（詳細については、[DNS設定](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/launch/checklist){target="_blank"}を参照）。

1. DNS サービスにアクセスし、各ドメインおよびホスト名のA レコードおよびCNAME レコードを更新します。
   1. _prod.magentocloud.map.fastly.net_&#x200B;を指す&#x200B;**&lt;&lt;www.yourdomain.com>>**&#x200B;のCNAME レコードを追加します
   2. _&lt;&lt;yourdomain.com>>_&#x200B;の4つのA レコードを設定します。次の場所を指します。\
      151.101.1.124\
      151.101.65.124\
      151.101.129.124\
      151.101.193.124
2. Adobe Commerce ベース URLを&#x200B;_に変更します&lt;&lt;www.yourdomain.com>>_
3. TTL時間が経過するのを待ってから、web ブラウザーを再起動します。
4. web サイトのテスト：

### 運用開始をブロックする際に問題が発生した場合：

カットオーバー中に起動できない問題が発生した場合、タイムリーなサポートを受けるには、ヘルプデスクを使用して「ストアを起動できません」という理由でチケットを開き、ホットラインサポート番号を電話で問い合わせるのが最も簡単です（現在の番号と手順については、[Adobe Commerce P1 Notification Hotline](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-p1-notification-hotline){target="_blank"}を参照）。

* 米国フリーダイヤル：（+1） 877 282 7436 （Adobe Commerce P1 ホットライン直通）
* 米国フリーダイヤル：（+1） 800 685 3620 （最初のメニューで、Adobe Commerce P1 ホットラインの場合は7 キーを押してください）
* 米国ローカル：（+1） 408 537 8777

## &#x200B;11. 運用開始後

サイトが公開されたら、割り当てられたCTA（カスタマーテクニカルアドバイザー）、CSE （カスタマーサクセスエンジニア）、AM （アカウントマネージャー）にメールを送信します。 ただし、プロジェクトにアカウントマネージャーが割り当てられていない場合は、サイトの稼働開始後にHigh SLA モニタリングを有効にするように求めるサポートチケットを作成できます。 CTA/CSEは、サイトがFastlyを有効にしてキャッシュを使用してローンチされたことを確認するとすぐに、次のタスクを実行します。

* クラスターにライブのタグを付け、サポートチケットを作成して、高SLA（サービスレベル契約）モニタリングを有効にします。
* アップタイムモニタリング用にNew Relic Syntheticsをアクティブ化します。

>[!MORELIKETHIS]
> 
> * [Launchの準備状況の概要 – 実装プレイブック ](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/launch/overview){target="_blank"}
> * [ チェックリストを起動 – ユーザーガイド ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/launch/checklist){target="_blank"}
> * [Prelaunch Checklist - Site Manager/Commerce管理者ガイド ](https://experienceleague.adobe.com/en/docs/commerce-admin/start/setup/prelaunch-checklist){target="_blank"}
> * [共有責任セキュリティ モデル ](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"}

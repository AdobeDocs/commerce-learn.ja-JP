---
title: Adobe Commerce Cloud の起動前のチェックリスト
description: Adobe Commerce Cloud のローンチ前のチェックリストについて説明します。
feature: Cloud
topic: Commerce, Architecture, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-04-17T00:00:00Z
jira: KT-15180
kt: 15180
exl-id: c6adb2c2-f194-4a3d-9290-e0837ef062ae
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '1668'
ht-degree: 0%

---

# Commerce Cloudの事前起動チェックリスト

Adobe Commerceの概要を次に示します [Site Launch ドキュメント &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/launch/overview){target="_blank"}。

このチェックリストは、Adobe Commerce Cloud サイトのローンチを成功に導くための計画と実行に役立ちます。 Adobe Commerce Cloud のシステムインテグレーターと協力して、すべての設定タスクとチェックリスト項目が完了および検証されていることを確認します。 チェックリスト項目に関して問題が発生した場合、または質問がある場合は、指定されたカスタマーテクニカルアドバイザーまたはカスタマーサクセスエンジニアにお問い合わせください。 アカウントにCTA/CSE が割り当てられていない場合は、サポートチケットを作成してください。

アカウントにCTA/CSE が割り当てられている場合は、新しい Adobe Commerce Cloud サイトを立ち上げる 4 週間前までに、立ち上げの **意図** を通知するためにそれらのユーザーとアカウントマネージャーに連絡してください。

- 一部のチェックは、慎重にレビューされない場合に運用開始をブロックする可能性があるので、[!BADGE &#x200B; ブロッカー &#x200B;]{type=caution tooltip="潜在的遮断薬"} でハイライト表示されます。
- お客様の実装アプローチに合わせて、開発者またはシステム統合パートナーとの共同作業を確実に行います。

>[!IMPORTANT]
> このチェックリストを使用および完了できない場合、実稼動の開始スケジュールおよび継続的なサイトの安定性に対する悪影響および関連するリスクについて、お客様は [&#x200B; 責任 &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"} を受け入れます。

## 1.運用開始前

1. テストと運用に関するドキュメントを確認する [&#x200B; サイト開始に関するドキュメント &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/launch/overview){target="_blank"}

   >[!NOTE]
   >パートナーまたはシステムインテグレーターと共に _必要なすべてのアクション項目を組み込んだ、包括的な_ 「運用開始準備計画」を準備します。 ローンチ前のチェックリストではAdobeのベストプラクティスが重視されますが、独自の運用開始準備計画の必要性は _&#x200B;**ありません**&#x200B;_。

2. [!BADGE &#x200B; ブロッカー &#x200B;]{type=caution tooltip="潜在的遮断薬"} サポートインサイト（SWAT）の推奨事項と情報（[&#x200B; ユーザーガイド &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-operations/tools/site-wide-analysis-tool/intro){target="_blank"}）を確認する
3. エンドユーザー/マーチャントが、バックエンド操作を含む UAT （ユーザー受け入れテスト）を実施しました。
4. システムインテグレーターチームは、ステージングと実稼動でエンドツーエンドの UAT を実行しました。 [Experience League ドキュメント &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/develop/test/staging-and-production){target="_blank"} を参照してください。
5. ステージング環境と実稼動環境でのコードのデプロイメントとテストを確認します（[&#x200B; 詳細を表示 &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/develop/test/staging-and-production){target="_blank"}）。
6. 実稼動クラスターは、契約された毎日のベースラインに合わせて永続的にサイズ変更されています。 詳しくは、割り当てられたCTA/CSE にお問い合わせいただくか、サポートチケットを発行してください。

## 2.現在の構成

1. Adobe Commerceおよび関連するパッケージ/サービスの [&#x200B; 最新バージョン &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-operations/release/notes/overview){target="_blank"} へのアップグレード
2. SI/パートナーと現在の構成およびサービスを確認し、[&#x200B; ベストプラクティスに従う &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-operations/implementation-playbook/best-practices/planning/catalog-management){target="_blank"}。
3. MySQL/Shared-Files [&#x200B; ディスク使用量 &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/develop/storage/manage-disk-space){target="_blank"} を確認します。

## &#x200B;3. Fastly の設定

1. [!BADGE &#x200B; ブロッカー &#x200B;]{type=caution tooltip="潜在的遮断薬"} キャッシュが機能していること（[&#x200B; フルページキャッシュ &#x200B;](https://developer.adobe.com/commerce/frontend-core/guide/caching/){target="_blank"} または [GraphQLのキャッシュ &#x200B;](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}）を確認します。 [Fastly セットアップガイド &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/cdn/fastly){target="_blank"} を参照してください。
2. 該当する場合は、PWA/ヘッドレス web サイトでGraphQL クエリにGET メソッドを使用します。

   >[!NOTE]
   > キャッシュできるのは、HTTP GET操作で送信されたクエリのみです（該当する場合）。 [POST クエリはキャッシュできません &#x200B;](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}。

3. Fastly 画像の最適化が有効になっていることを確認します（[Fastly 画像の最適化を参照 &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/cdn/fastly-image-optimization){target="_blank"}）。
4. シールドの場所が正しく設定されていることを確認します（[&#x200B; キャッシュ、バックエンド、およびオリジン シールドを設定 &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/cdn/setup-fastly/fastly-custom-cache-configuration){target="_blank"}）。
5. Web アプリケーション ファイアウォール （**WAF**）が動作しています。 （[&#x200B; ブロックされたリクエストのトラブルシューティング &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/cdn/fastly-waf-service){target="_blank"} （ある場合）と制限事項を参照してください）。
6. 管理パネルの Fastly [&#x200B; 「無視された URL パラメーター」 &#x200B;](https://github.com/iancassidyweb/magento2/commit/68fdecfcd26c957382b8d68b64887e0a83298524){target="_blank"} リストを更新して、キャッシュのパフォーマンスを強化します。

   >[!NOTE]
   > _管理者/ストア/設定/システム/フルページキャッシュ/Fastly 設定/詳細設定/無視された URL パラメーター（グローバル）_ の下の Fastly 設定で、キャッシュされたページを検索する際に Fastly で無視するパラメーターのコンマ区切りリストを見つけることができます。 このリストを変更した後は、VCL を再度アップロードしてください

## &#x200B;4. DNS と SSL

1. [!BADGE &#x200B; ブロッカー &#x200B;]{type=caution tooltip="潜在的遮断薬"} 必要なすべてのドメイン名が要求されていることを確認します。 _（追加または変更されたドメインに対して事前にサポートチケットを送信）_
2. [!BADGE &#x200B; ブロッカー &#x200B;]{type=caution tooltip="潜在的遮断薬"} ドメインに SSL （TLS）証明書が適用されています。 詳しくは、[&#x200B; この記事 &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-knowledge-base/kb/how-to/ssl-tls-certificates-for-magento-commerce-cloud-faq){target="_blank"} を参照してください。
3. 運用開始のために、DNS[TTL （Time to Live） &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/launch/checklist#to-update-dns-configuration-for-site-launch){target="_blank"} 値を可能な限り最小限に更新します。
4. Sendgrid SPF とDKIMを有効にする

   >[!NOTE]
   > 各ドメインの SendGrid CNAME レコードを DNS 設定に追加します。 送信者ドメインの変更方法などを確認するには、[SendGrid メールサービス &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/project/sendgrid){target="_blank"} をお読みください。

## &#x200B;5. データベース構成

Adobe Commerce Cloud では、ステージング環境と実稼動環境の両方で MariaDB Galera クラスターをデータベースとして使用します。 Galera クラスターは、パフォーマンスとスケーラビリティの向上に役立ちます。 Galera クラスタ・レプリケーションの最適なプラクティスと制約に関するインサイトを得るには、次の記事を参照してください。

- [MySQL 設定のベストプラクティス &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration){target="_blank"}
- Adobe Commerceの管理アラート：[MariaDB アラート &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-mariadb-alerts){target="_blank"}
- [&#x200B; データベース設定 &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud){target="_blank"} のベストプラクティス
- [Galera クラスタ・レプリケーションとフロー制御 &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-learn/tutorials/backend-development/galera-db-slow-replication){target="_blank"} の詳細な分析

1. 高いデータベース負荷の際のパフォーマンスを向上させるために、[MYSQL スレーブ接続 &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration#slave-connections){target="_blank"} をお勧めします。
2. すべてのデータベーステーブルの行形式が、COMPACT ではなく [DYNAMIC](https://experienceleague.adobe.com/ja/docs/commerce-operations/implementation-playbook/best-practices/maintenance/mariadb-upgrade#convert-database-table-storage-format){target="_blank"} に設定されていることを確認します（これは、オンプレミスからクラウドへの移行に特に当てはまります）。
3. すべてのテーブルについて、データベースストレージエンジンを [MyISAM から InnoDB](https://experienceleague.adobe.com/ja/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud#convert-all-myisam-tables-to-innodb){target="_blank"} に変更します。
4. 1 GB を超えるデータベーステーブルを事前に確認して最適化します。
5. データベーススキーマ情報は最新です。 （[&#x200B; このガイド &#x200B;](https://mariadb.com/kb/en/engine-independent-table-statistics/#collecting-statistics-with-the-analyze-table-statement){target="_blank"} を参照してください）。

## &#x200B;6. デプロイメント

1. 実稼動環境へのデプロイメント時のメンテナンス時間を短縮するには、静的コンテンツデプロイメント（SCD）の理想的な状態を確認します。 [&#x200B; 静的コンテンツデプロイメント（SCD）戦略 &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/develop/deploy/static-content){target="_blank"} および [&#x200B; ストア設定管理 &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/configure-store/store-settings){target="_blank"} ガイドを確認してください。
2. HTML、JavaScriptおよび CSS の縮小設定を確認します。 （これはPWA/ヘッドレス web サイトには適用されません）。
3. 次のクラウド変数の使用が、その意図した目的に合っていることを確認します。 （[SCD_MATRIX](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-build#scd_matrix){target="_blank"}、[SCD_ON_DEMAND](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-global#scd_on_demand){target="_blank"} および [SKIP_SCD](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-deploy#skip_scd){target="_blank"}）

## &#x200B;7. テストとトラブルシューティング

1. 送信トランザクションメールをテストします。 詳しくは、[Adobe Commerce Cloud - SendGrid Mail 機能 &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/project/sendgrid){target="_blank"} を参照してください。
2. [!BADGE &#x200B; ブロッカー &#x200B;]{type=caution tooltip="潜在的遮断薬"} Adobeを使用しているブロッカーはありますか？
3. [!BADGE &#x200B; ブロッカー &#x200B;]{type=caution tooltip="潜在的遮断薬"} 運用開始前に実稼動インスタンスで負荷テストとストレステストを実行し、割り当てられたCTA/CSE と結果を共有します。

   >[!NOTE]
   > [&#x200B; 負荷テストとストレステストは、ボトルネックを特定し &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/develop/test/guidance#:~:text=A%20load%20test%20can%20help,Scan%20Tool%20for%20your%20sites.){target="_blank"} アプリケーション内のパフォーマンスの問題を明らかにするという目的に役立ちます。 クラスターサイズに関する期待値を管理し、ビジネス要件を効果的に満たすために必要なスケーリング調整を決定する上で重要な役割を果たします。

   >[!IMPORTANT]
   > **_WARNING:_** 負荷テストを準備するときは、ライブトランザクションメールを送信してください **_送信しないでください_** （ダミーアドレスも含む）。 テスト中にメールを送信すると、プロジェクトがローンチ前に SendGrid に設定されたデフォルトの送信制限（12k）に達する可能性があります。
   > 
   > - メール通信を無効にする方法：
   >   _ストア/設定/詳細/システム/メール送信設定_ に移動します。

4. [&#x200B; 共有責任セキュリティモデル &#x200B;](https://business.adobe.com/jp/products/magento/secure-ecommerce.html){target="_blank"} の一部として、実稼動インスタンスでセキュリティ侵入テストを実施します。 PCI （Payment Card Industry）コンプライアンスのために、カスタマイズされたサイトには侵入テストが必要です。

## 8.その他の構成

1. インデックス作成を _スケジュールに従って更新_」に切り替えます。ただし、**_customer_grid_** は「保存」のままです（[&#x200B; インデックス作成モード &#x200B;](https://developer.adobe.com/commerce/php/development/components/indexing/#indexing-modes){target="_blank"} を参照）。
2. サードパーティの検索エンジンまたは拡張機能を使用していますか？
3. 関連する場合は、インデクサー/クローラーで web サイトをスキャンできるように [&#128279;](https://experienceleague.adobe.com/ja/docs/commerce-admin/marketing/seo/seo-overview){target="_blank"}SEO （検索エンジン最適化）設定が適切にセットアップされていることを確認します。
4. リダイレクトとルートの追加（[&#x200B; ルートの設定 &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/configure/routes/routes-yaml){target="_blank"} を参照）

   >[!NOTE]
   >統合環境で routes.yaml ファイルへのリダイレクトとルートを追加し、ステージングと実稼動にデプロイする前に、この環境での設定を確認します。

       &quot;http://{all}/&quot;:
        タイプ : アップストリーム 
        アップストリーム：「mymagento:http」 
       
       &quot;http://{all}/&quot;:
        タイプ : アップストリーム 
        アップストリーム：「mymagento:http」 
   
5. 開発時に XDebug が有効な場合は、必ず無効にします（[Xdebug の設定 &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/){target="_blank"} を参照）。
6. op-cache およびその他の設定が php.ini ファイル内で正しく更新されていることを確認します（[&#x200B; このサンプルを参照 &#x200B;](https://github.com/magento/magento-cloud/blob/master/php.ini#L41){target="_blank"}）。
7. [**Adobe Commerce ステータスページ**](https://status.adobe.com/cloud/experience_cloud#/){target="_blank"} を購読します。
8. New Relicの「[Adobe Commerceのマネージドアラート &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce){target="_blank"}」通知チャネルを購読して、特定のパフォーマンス指標を監視する（[&#x200B; 詳細情報 &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/monitor/new-relic/new-relic-service){target="_blank"}）。

## 9.安全保障

1. Adobe Commerce セキュリティスキャンの設定

   >[!NOTE]
   > [Adobe Commerce セキュリティ スキャンは &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-admin/systems/security/security-scan){target="_blank"} サイト上の古いソフトウェア バージョン、不適切な構成、および潜在的なマルウェアを検出するのに役立つ便利なツールです。 新規登録し、頻繁に実行するようにスケジュールして、メールが適切なセキュリティ技術担当者に送信されるようにします。
   > 
   > UAT 中にこのタスクを完了します。 定期的なスキャン オプションを使用する場合は、必ず低需要時にスキャンをスケジュールしてください。 詳しくは、Adobe Commerce アカウントの [&#x200B; セキュリティスキャン &#x200B;](https://account.magento.com/scanner/index/dashboard/){target="_blank"} ページを参照してください。 セキュリティスキャンにアクセスするには、Adobe Commerce アカウントにログインする必要があります。

2. Adobe Commerce Admin のデフォルト設定を変更します。
3. 管理者パスワードを変更します（[Admin Security の設定 &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-admin/systems/security/security-admin){target="_blank"} を参照）。
4. 管理 URL を変更します（[&#x200B; カスタム管理 URL の使用 &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-admin/stores-sales/site-store/store-urls#use-a-custom-admin-url){target="_blank"} を参照）。
5. プロジェクトに属さなくなったユーザーを削除します（[&#x200B; ユーザーの作成と管理 &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/project/user-access){target="_blank"} を参照）。
6. 管理者用のパスワードが設定されている（[&#x200B; 管理者パスワードの要件 &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-admin/systems/security/security-admin){target="_blank"} を参照）。
7. 二要素認証を設定します（[&#x200B; 二要素認証 &#x200B;](https://developer.adobe.com/commerce/testing/functional-testing-framework/two-factor-authentication/){target="_blank"} を参照）。

## 10.運用開始

カットオーバーの段階になったら、次の手順を実行してください（詳しくは [DNS 設定 &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/launch/checklist){target="_blank"} を参照してください）。

1. DNS サービスにアクセスし、各ドメインとホスト名の A レコードと CNAME レコードを更新します。
   1. _&lt;&lt;www.yourdomain.com>>_ の CNAME レコードを追加します。これは **prod.magentocloud.map.fastly.net** を指します
   2. _&lt;&lt;yourdomain.com>>_ に 4 つの A レコードを設定します。\
      151.101.1.124\
      151.101.65.124\
      151.101.129.124\
      151.101.193.124
2. Adobe Commerceのベース URL を _&lt;&lt;www.yourdomain.com>>_ に変更します。
3. TTL 時間が経過するのを待ってから、web ブラウザーを再起動します。
4. Web サイトをテストします。

### 運用開始をブロックする問題がある場合：

カットオーバー中に起動できない問題が発生した場合は、ヘルプデスクを利用し、「店舗を起動できません」という理由でチケットを開き、ホットラインサポート番号を呼び出すことで、適切なタイムリーなサポートを受けることができます（[Adobe Commerce P1 （優先度 1）ホットライン番号のリスト &#x200B;](https://support.magento.com/hc/en-us/articles/360042536151){target="_blank"}）。

- 米国フリーダイヤル：（+1） 877 282 7436 （Adobe Commerce P1 ホットライン直通）
- 米国フリーダイヤル：（+1） 800 685 3620 （最初のメニューは、Adobe Commerce P1 ホットラインの場合は 7 を押してください）
- 米国ローカル：（+1） 408 537 8777

## 11.運用開始後

サイトが公開されたら、割り当てられたCTA（カスタマーテクニカルアドバイザリ）、CSE （カスタマーサクセスエンジニア）および AM （アカウントマネージャー）にメールで問い合わせます。 ただし、アカウントマネージャーがプロジェクトに割り当てられていない場合は、サイトが稼働したら高SLAモニタリングを有効にするよう求めるサポートチケットを作成できます。 CTA/CSE は、Fastly を有効にしてキャッシングによるサイトの起動が確認され次第、次のタスクを実行します。

- クラスターをライブとしてタグ付けし、サポートチケットを作成して、高SLA（サービスレベル契約）の監視を有効にします。
- 稼動時間の監視のためにNew Relic Synthetics を有効にします。

>[!MORELIKETHIS]
> 
> - [Launch 対応の概要 – 実装プレイブック &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-operations/implementation-playbook/best-practices/launch/overview){target="_blank"}
> - [Launch チェックリスト – ユーザーガイド &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/launch/checklist){target="_blank"}
> - [Prelaunch チェックリスト - Site Manager/Commerce管理者ガイド &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-admin/start/setup/prelaunch-checklist){target="_blank"}
> - [&#x200B; 共有責任セキュリティ・モデル &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"}

---
title: Adobe Commerce Cloudの起動前のチェックリスト
description: Adobe Commerce Cloudの事前起動チェックリストについて説明します。
feature: Cloud
topic: Commerce, Architecture, Development
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-04-17T00:00:00Z
jira: KT-15180
kt: 15180
exl-id: c6adb2c2-f194-4a3d-9290-e0837ef062ae
source-git-commit: 10b6f979e9fd1e6782c38c4730a679bc22f25eea
workflow-type: tm+mt
source-wordcount: '1604'
ht-degree: 0%

---

# Commerce Cloudの起動前のチェックリスト

Adobe Commerceの概要を次に示します [サイトのローンチドキュメント](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/launch/overview){target="_blank"}.

このチェックリストは、Adobe Commerce Cloud サイトのローンチを成功に導くための計画と実行に役立ちます。 Adobe Commerce Cloudのシステムインテグレーターと協力して、すべての設定タスクとチェックリスト項目が完了および検証されていることを確認します。 チェックリスト項目に関して問題が発生した場合、または質問がある場合は、指定されたカスタマーテクニカルアドバイザーまたはカスタマーサクセスエンジニアにお問い合わせください。 アカウントに CTA/CSE が割り当てられていない場合は、サポートチケットを作成してサポートを受けることができます。

アカウントに CTA/CSE が割り当てられている場合は、新しいAdobe Commerce Cloud サイトを立ち上げる 4 週間前までに、お客様と担当営業にお問い合わせください。 **故意** を起動します。

- 一部のチェックは、でハイライト表示されます [!BADGE ブロッカー]{type=caution tooltip="潜在的遮断薬"}
- お客様の実装アプローチに合わせて、開発者またはシステム統合パートナーとの共同作業を確実に行います。

>[!IMPORTANT]
> 同意します [責務](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"} 実稼動ローンチスケジュールおよび継続的なサイトの安定性に対する悪影響やそれに関連するリスクについては、このチェックリストを使用して完了できなかった場合に発生します。

## 1.運用開始前

1. テストと運用の開始に関するドキュメントを確認する [サイトのローンチドキュメント](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/launch/overview){target="_blank"}

   >[!NOTE]
   >包括的な _「運用開始準備計画」_ は、必要なアクション項目をすべて組み込んで、パートナーまたはシステムインテグレーターと完全に準備されています。 ローンチ前のチェックリストではAdobeのベストプラクティスを重視していますが、 _**次を含まない**_ 独自の運用開始準備計画の必要性を置き換えます。

2. [!BADGE ブロッカー]{type=caution tooltip="潜在的遮断薬"}[ユーザーガイド](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/intro){target="_blank"}）
3. エンドユーザー/マーチャントが、バックエンド操作を含む UAT （ユーザー受け入れテスト）を実施しました。
4. システムインテグレーターチームは、ステージングと実稼動でエンドツーエンドの UAT を実行しました。 を参照してください。 [Experience Leagueドキュメント](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/develop/test/staging-and-production){target="_blank"}.
5. ステージング環境と実稼動環境でのコードのデプロイメントとテストを確認します（[詳細情報](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/develop/test/staging-and-production){target="_blank"}）に設定します。
6. 実稼動クラスターは、契約された毎日のベースラインに合わせて永続的にサイズ変更されています。 詳しくは、割り当てられた CTA/CSE にお問い合わせいただくか、サポートチケットを発行してください。

## 2.現在の構成

1. Adobe Commerceおよび関連するパッケージ/サービスの、へのアップグレード [最新バージョン](https://experienceleague.adobe.com/en/docs/commerce-operations/release/notes/overview){target="_blank"}
2. SI/パートナーと現在の構成およびサービスを確認する [ベストプラクティスに従う](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/catalog-management){target="_blank"}.
3. MySQL/Shared-Files のレビュー [ディスク使用量](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/develop/storage/manage-disk-space){target="_blank"}

## 3. Fastly の設定

1. [!BADGE ブロッカー]{type=caution tooltip="潜在的遮断薬"}[フルページキャッシュ](https://developer.adobe.com/commerce/frontend-core/guide/caching/){target="_blank"} または [GraphQLのキャッシュ](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}）に設定します。 を読み取る [Fastly セットアップガイド](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/cdn/fastly){target="_blank"}.
2. 該当する場合は、PWA/ヘッドレス web サイトでのGraphQL クエリのGET方式を使用します。

   >[!NOTE]
   > キャッシュできるのは、HTTP GET操作で送信されたクエリのみです（該当する場合）。 [POSTクエリはキャッシュできません](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}.

3. Fastly での画像の最適化が有効になっていることを確認します（[Fastly での画像の最適化を参照してください](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/cdn/fastly-image-optimization){target="_blank"}）
4. シールドの正しい位置が設定されていることを確認します（[キャッシュ、バックエンド、オリジンシールドの設定](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/cdn/setup-fastly/fastly-custom-cache-configuration){target="_blank"}）に設定します。
5. Web アプリケーションファイアウォール （**WAF**）が動作しています。 （詳しくは、 [ブロックされたリクエストのトラブルシューティング](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/cdn/fastly-waf-service){target="_blank"}（存在する場合は）と制限
6. Fastly の更新 [「無視された URL パラメーター」](https://github.com/iancassidyweb/magento2/commit/68fdecfcd26c957382b8d68b64887e0a83298524){target="_blank"} 管理パネルのリストで、キャッシュのパフォーマンスを強化します。

   >[!NOTE]
   > の下の Fastly 設定で _管理者/ ストア /設定/ システム /完全なページキャッシュ / Fastly 設定/詳細設定/無視された URL パラメーター（グローバル）_&#x200B;には、キャッシュされたページを検索する際に Fastly で無視する必要があるパラメーターのコンマ区切りリストを用意しています。 このリストを変更した後は、VCL を再度アップロードしてください

## 4. DNS と SSL

1. [!BADGE ブロッカー]{type=caution tooltip="潜在的遮断薬"}_（ドメインを追加または変更する場合は、事前にサポートチケットを送信します）_
2. [!BADGE ブロッカー]{type=caution tooltip="潜在的遮断薬"}[この記事](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/ssl-tls-certificates-for-magento-commerce-cloud-faq){target="_blank"} を参照してください。
3. DNS を更新 [TTL （有効期間）](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/launch/checklist#to-update-dns-configuration-for-site-launch){target="_blank"} 運用開始のためにできるだけ小さい値に設定します。
4. Sendgrid SPF と DKIM の有効化

   >[!NOTE]
   > 各ドメインの SendGrid CNAME レコードを DNS 設定に追加します。 Read [SendGrid メールサービス](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/project/sendgrid){target="_blank"} 送信者ドメインの変更方法などを確認するには、を参照してください。

## 5. データベース構成

Adobe Commerce Cloudでは、ステージング環境と実稼動環境の両方で MariaDB Galera クラスターをデータベースとして使用しています。 Galera クラスターは、パフォーマンスとスケーラビリティの向上に役立ちます。 Galera クラスタ・レプリケーションの最適なプラクティスと制約に関するインサイトを得るには、次の記事を参照してください。

- [MySQL 設定のベストプラクティス](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration){target="_blank"}
- Adobe Commerceの管理アラート： [MariaDB アラート](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-mariadb-alerts){target="_blank"}
- のベストプラクティス [データベース設定](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud){target="_blank"}
- ～に対する深い分析 [Galera クラスタ・レプリケーションとフロー制御。](https://experienceleague.adobe.com/en/docs/commerce-learn/tutorials/backend-development/galera-db-slow-replication){target="_blank"}

1. [MYSQL スレーブ接続](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration#slave-connections){target="_blank"} は、データベースの高負荷時のパフォーマンスを向上させるために推奨されます。
2. すべてのデータベーステーブルの行形式がに設定されていることを確認します。 [コンパクトではなく動的](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/maintenance/mariadb-upgrade#convert-database-table-storage-format){target="_blank"} （これは、オンプレミスからクラウドへの移行に特に当てはまります）。
3. データベースストレージエンジンの変更元 [MyISAM から InnoDB へ](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud#convert-all-myisam-tables-to-innodb){target="_blank"} すべてのテーブル用。
4. 1 GB を超えるデータベーステーブルを事前に確認して最適化します。
5. データベーススキーマ情報は最新です。 （を参照）。 [このガイド](https://mariadb.com/kb/en/engine-independent-table-statistics/#collecting-statistics-with-the-analyze-table-statement){target="_blank"}）に設定します。

## 6. デプロイメント

1. 実稼動環境へのデプロイメント時のメンテナンス時間を短縮するには、静的コンテンツデプロイメント（SCD）の理想的な状態を確認します。 レビュー [静的コンテンツデプロイメント（SCD）戦略](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/develop/deploy/static-content){target="_blank"} および [ストアの設定管理](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure-store/store-settings){target="_blank"} ガイド。
2. HTML、JavaScript および CSS の縮小設定を確認します。 （これは、PWA/ヘッドレス web サイトには適用されません）。
3. 次のクラウド変数の使用が、その意図した目的に合っていることを確認します。 （[SCD_MATRIX](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-build#scd_matrix){target="_blank"}, [SCD_ON_DEMAND](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-global#scd_on_demand){target="_blank"} および [SKIP_SCD](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-deploy#skip_scd){target="_blank"}）

## 7. テストとトラブルシューティング

1. 送信トランザクションメールをテストします。 詳細を読む： [Adobe Commerce Cloud - SendGrid Mail 機能](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/project/sendgrid){target="_blank"}.
2. [!BADGE ブロッカー]{type=caution tooltip="潜在的遮断薬"}
3. [!BADGE ブロッカー]{type=caution tooltip="潜在的遮断薬"}

   >[!NOTE]
   > A [負荷テストとストレステストが目的です](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/develop/test/guidance#:~:text=A%20load%20test%20can%20help,Scan%20Tool%20for%20your%20sites.){target="_blank"} ボトルネックを特定し、アプリケーション内のパフォーマンスの問題を明らかにする クラスターサイズに関する期待値を管理し、ビジネス要件を効果的に満たすために必要なスケーリング調整を決定する上で重要な役割を果たします。

   >[!IMPORTANT]
   > **_警告：_** 負荷テストを準備するときは、_してください **_実行しない_** ライブトランザクションのメールを送信します（ダミーアドレスにも送信）。 テスト中にメールを送信すると、プロジェクトがローンチ前に SendGrid に設定されたデフォルトの送信制限（12k）に達する可能性があります。
   > 
   > - メール通信を無効にする方法：
   >   に移動 _ストア /設定/詳細/ システム / E メール送信設定_.

4. の一部として、実稼動インスタンスでセキュリティ侵入テストを実施します。 [共有責任セキュリティ モデル](https://business.adobe.com/products/magento/secure-ecommerce.html){target="_blank"}. PCI （Payment Card Industry）コンプライアンスのために、カスタマイズされたサイトには侵入テストが必要です。

## 8.その他の構成

1. インデックスをに切り替え _&quot;スケジュールに従って更新_``、ただし **_customer_grid_** （「SAVE」に残る）（ [インデックス作成モード](https://developer.adobe.com/commerce/php/development/components/indexing/#indexing-modes){target="_blank"}）に設定します。
2. サードパーティの検索エンジンまたは拡張機能を使用していますか？
3. を確認します。 [SEO （検索エンジン最適化）の設定が適切にセットアップされている](https://experienceleague.adobe.com/en/docs/commerce-admin/marketing/seo/seo-overview){target="_blank"} 関連する場合に、インデクサー/クローラーで web サイトのスキャンを有効にします。
4. リダイレクトとルートの追加（を参照） [ルートの設定](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure/routes/routes-yaml){target="_blank"}）

   >[!NOTE]
   >統合環境で routes.yaml ファイルへのリダイレクトとルートを追加し、ステージングと実稼動にデプロイする前に、この環境での設定を確認します。

       「http://{all}/&quot;:
       タイプ：アップストリーム
       アップストリーム：「mymagento:http」
       
       「http://{all}/&quot;:
       タイプ：アップストリーム
       アップストリーム：「mymagento:http」
   
5. 開発時に XDebug が有効な場合は、必ず無効にします（ [Xdebug の設定](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/){target="_blank"}）に設定します。
6. op-cache およびその他の設定が php.ini ファイルで正しく更新されていることを確認します（[このサンプルを参照してください](https://github.com/magento/magento-cloud/blob/master/php.ini#L41){target="_blank"}）に設定します。
7. を購読 [**Adobe Commerce ステータスページ**](https://status.adobe.com/cloud/experience_cloud#/){target="_blank"}.
8. New Relicに登録」[Adobe Commerceの管理アラート](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce){target="_blank"}指定されたパフォーマンス指標を監視する通知チャネル （[詳細を読む](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/monitor/new-relic/new-relic-service){target="_blank"}）に設定します。

## 9.安全保障

1. Adobe Commerce セキュリティスキャンの設定

   >[!NOTE]
   > [Adobe Commerce セキュリティスキャンは便利なツールです](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-scan){target="_blank"} これにより、古いソフトウェアバージョン、誤った設定、サイト上の潜在的なマルウェアを検出できます。 新規登録し、頻繁に実行するようにスケジュールして、メールが適切なセキュリティ技術担当者に送信されるようにします。
   > 
   > UAT 中にこのタスクを完了します。 定期的なスキャン オプションを使用する場合は、必ず低需要時にスキャンをスケジュールしてください。 を参照してください。 [セキュリティスキャン](https://account.magento.com/scanner/index/dashboard/){target="_blank"} Adobe Commerce アカウント内のページ。 セキュリティスキャンにアクセスするには、Adobe Commerce アカウントにログインする必要があります。

2. Adobe Commerce Admin のデフォルト設定を変更します。
3. 管理者パスワードを変更する（を参照） [Admin Security の設定](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-admin){target="_blank"}）に設定します。
4. 管理者 URL を変更する（ [カスタム管理 URL の使用](https://experienceleague.adobe.com/en/docs/commerce-admin/stores-sales/site-store/store-urls#use-a-custom-admin-url){target="_blank"}）に設定します。
5. プロジェクトに属さなくなったユーザーを削除します（ [ユーザーの作成と管理](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/project/user-access){target="_blank"}）に設定します。
6. 管理者用のパスワードが設定されている（ [管理者パスワードの要件](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-admin){target="_blank"}）に設定します。
7. 二要素認証の設定（を参照） [二要素認証](https://developer.adobe.com/commerce/testing/functional-testing-framework/two-factor-authentication/){target="_blank"}）に設定します。

## 10.運用開始

カットオーバーの時期が来たら、次の手順を実行してください（詳しくは、 [DNS 設定](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/launch/checklist){target="_blank"}）:

1. DNS サービスにアクセスし、各ドメインとホスト名の A レコードと CNAME レコードを更新します。
   1. の CNAME レコードを追加 _&lt;&lt;www.yourdomain.com>>_、を指す **prod.magentocloud.map.fastly.net**
   2. に 4 つの A レコードを設定 _&lt;&lt;yourdomain.com>>_&#x200B;を指しています。\
      151.101.1.124\
      151.101.65.124\
      151.101.129.124\
      151.101.193.124
2. Adobe Commerce ベース URL をに変更します _&lt;&lt;www.yourdomain.com>>_
3. TTL 時間が経過するのを待ってから、web ブラウザーを再起動します。
4. Web サイトをテストします。

### 運用開始をブロックする問題がある場合：

カットオーバー中に起動できない問題が発生した場合は、ヘルプデスクを利用し、「ストアを起動できません」という理由でチケットを開き、ホットラインサポート番号に電話します（ [Adobe Commerce P1 （優先度 1）ホットライン番号のリスト](https://support.magento.com/hc/en-us/articles/360042536151){target="_blank"}）:

- 米国フリーダイヤル：（+1） 877 282 7436 （Adobe Commerce P1 ホットライン直通）
- 米国フリーダイヤル：（+1） 800 685 3620 （最初のメニューは、Adobe Commerce P1 ホットラインの場合は 7 を押してください）
- 米国ローカル：（+1） 408 537 8777

## 11.運用開始後

サイトが公開されたら、割り当てられた CTA （カスタマーテクニカルアドバイザリ）、CSE （カスタマーサクセスエンジニア）および AM （アカウントマネージャー）にメールで問い合わせます。 ただし、アカウントマネージャーがプロジェクトに割り当てられていない場合は、サイトが稼働したら高 SLA 監視を有効にするよう求めるサポートチケットを作成できます。 Cta/CSE は、Fastly を有効にしてキャッシュすることでサイトの起動が確認されると、直ちに次のタスクを実行します。

- クラスターをライブとしてタグ付けし、サポートチケットを作成して高 SLA （サービスレベル契約）監視を有効化します。
- 稼動時間監視のために Pingdom チェックを有効化します。

>[!MORELIKETHIS]
> 
> - [Launch 対応の概要 – 実装プレイブック](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/launch/overview){target="_blank"}
> - [Launch チェックリスト – ユーザーガイド](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/launch/checklist){target="_blank"}
> - [Prelaunch チェックリスト - Site Manager/Commerce管理者ガイド](https://experienceleague.adobe.com/en/docs/commerce-admin/start/setup/prelaunch-checklist){target="_blank"}
> - [共有責任セキュリティ モデル](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"}

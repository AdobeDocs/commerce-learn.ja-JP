---
title: データベースに対するクエリの接続と実行
description: Adobe Commerceクラウドプロジェクトに接続する方法をいくつか説明します。 オフサイトで使用するためにデータベースをプルダウンする方法を説明します。 PII をマスクして削除する方法を説明します。
feature: Backend Development,Console,Cloud
topic: Commerce,Development
role: Developer
level: Intermediate, Experienced
doc-type: Technical Video
duration: 0
last-substantial-update: 2024-02-14T00:00:00Z
jira: KT-14910
thumbnail: KT-14910.jpeg
exl-id: e740bbd0-5ec7-4272-89cb-0bed776eb149
source-git-commit: a951f61ff71ad3777f8aebfa3c237b2ec1a4b1a5
workflow-type: tm+mt
source-wordcount: '1047'
ht-degree: 0%

---

# Adobe Commerceデータベースに対するクエリの接続と実行

このチュートリアルでは、クラウドプロジェクト上のAdobe Commerceに接続し、オフサイトで使用するためにデータベースをダンプし、PII をマスクして削除する方法を学びます。

次のいずれかの方法を使用して、クラウドプロジェクトからAdobe Commerceデータにアクセスできます。

* ローカル DB ダンプの使用
* Mysql Workbench や Tables Plus などのアプリケーションを使用したリモートクラウド環境への DB 接続
* magento-cloud CLI ツールを使用してクラウド環境に直接接続し、リモートサーバーでコマンドを実行します。

お勧めの方法は、データベースダンプを実行し、それをスクラブして顧客情報を削除することです。 データが不要な場合は、顧客データを完全に削除します。

## Adobe Commerce Cloud CLI ツールの使用

データベースダンプを作成するには、 [Adobe Commerce Cloud CLI](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/dev-tools/cloud-cli/cloud-cli-overview.html) インストール済み ローカルのラップトップ上で、ディレクトリに移動し、次のコマンドを実行します。 必ず `your-project-id` プロジェクト ID を持つ（に似ている） `asasdasd45q`. また、 `your-environment-name` 環境の名前（例： ） `master` または `staging`.

`magento-cloud db:dump -p your-project-id -e your-environment-name`

プロジェクト ID や環境が不明な場合は、次のコマンドで省略できます。

`magento-cloud db:dump`

CLI で正しいプロジェクトと環境を指定するように求められます。 次の使用例は、このダイアログを表示します。 この例では、アカウントに割り当てられた複数のプロジェクトが表示されますが、1 つのプロジェクトしか使用できない可能性があります。

ディレクトリに変更

```bash
cd ~/Downloads/db-tutorial 
```

次に、コマンドを実行してデータベースダンプを作成します。

```bash
magento-cloud db:dump
```

プロジェクトや環境を指定していないので、Adobe Commerce CLI ではいくつかの質問が表示されます。次にダイアログの例を示します

```bash
Enter a number to choose a project:
  [0] demo-ralbin (ral32nryq4123)
  [1] adobe-commerce-demo (abc123zzkipexnqo)
  [2] DX Tutorials - Commerce (abasrpikfw4123)
 > 2

Enter a number to choose an environment:
Default: master
  [0] master (type: production)
  [1] remote-db (type: development)
 > 1

Creating SQL dump file: /Users/<username>/Downloads/db-tutorial/abasrpikfw4123--remote-db-ecpefky--mysql--main--dump.sql
```

## Adobe Commerce ECE-tools の使用

Adobe Commerce CLI ツールを使用していない場合は、 `ssh` をプロジェクトに追加して、を実行します。 `ece` command `vendor/bin/ece-tools db-dump`：レスポンスのサンプル：

```bash
ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud

 __  __                   _          ___ _             _ 
|  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
| |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
|_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
            |___/                                        

 Welcome to Magento Cloud.

 This is environment remote-db-ecpefky
 of project abasrpikfw4123.

web@mymagento.0:~$ vendor/bin/ece-tools db-dump
The db-dump operation switches the site to maintenance mode, stops all active cron jobs and consumer queue processes, and disables cron jobs before starting the dump process.
Your site will not receive any traffic until the operation completes.
Do you wish to proceed with this process? (y/N)?y
[2024-02-13T19:01:45.130999+00:00] INFO: Starting backup.
[2024-02-13T19:01:45.155039+00:00] NOTICE: Enabling Maintenance mode
[2024-02-13T19:01:46.404427+00:00] INFO: Trying to kill running cron jobs and consumers processes
[2024-02-13T19:01:46.420149+00:00] INFO: Running Magento cron and consumers processes were not found.
[2024-02-13T19:01:46.420434+00:00] INFO: Waiting for lock on db dump.
[2024-02-13T19:01:46.420499+00:00] INFO: Start creation DB dump for main database...
[2024-02-13T19:01:50.697886+00:00] INFO: Finished DB dump for main database, it can be found here: /app/var/dump-main-1707850906.sql.gz
[2024-02-13T19:01:51.628328+00:00] NOTICE: Maintenance mode is disabled.
[2024-02-13T19:01:51.628419+00:00] INFO: Backup completed.
web@mymagento.0:~$ exit
logout
Connection to ssh.us-4.magento.cloud closed.
```

用途 `SFTP` または `rsync` データベースダンプをローカル環境にプルする。

次の例では、 `rsync` ファイルを `~/Downloads/db-tutorial` フォルダー。

```bash
rsync -avrp -e ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud:/app/var/dump-main-1707850906.sql.gz ~/Downloads/db-tutorial
```

端末ウィンドウは情報を出力します。次に出力例を示します。

```bash
rsync -avrp -e ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud:/app/var/dump-main-1707850906.sql.gz ~/Downloads/db-tutorial
receiving file list ... done
dump-main-1707850906.sql.gz

sent 38 bytes  received 2691041 bytes  358810.53 bytes/sec
total size is 2690241  speedup is 1.00
```

ファイルの内容を表示して、ファイルが正常にダウンロードされたことを確認します。

```bash
ls -lah
total 29840
drwxr-xr-x    4 <ussername>  staff   128B Feb 13 13:02 .
drwx------@ 103 <ussername>   staff   3.2K Feb 13 12:52 ..
-rw-r--r--    1 <ussername>   staff    11M Feb 13 12:53 abasrpikfw4123--remote-db-ecpefky--mysql--main--dump.sql
-rw-r--r--    1 <ussername>   staff   2.6M Feb 13 13:01 dump-main-1707850906.sql.gz
```

データを取得したら、顧客データを削除またはマスクして、必ずクリーンアップします。 以下のサンプルスクリプトは、作業を開始する際に役立ちます。

この例では、顧客データをランダムな文字列に変換しますが、すべての項目を保持します。 この例には、顧客 PII がサードパーティのテーブルおよびコアテーブルで見つかることを示す追加のテーブルがいくつか含まれています。 各テーブルのデータを慎重に調べ、顧客データをマスクまたは削除します。

通常、アーキテクトまたはリード開発者は、データベースダンプのマスクとサニタイズを担当する唯一の人物です。 専用のサニタイザを持つことで、生データの露出が減り、コンプライアンスルールや規制に違反する機会が減ります。

```sql
SET FOREIGN_KEY_CHECKS=0;
UPDATE customer_entity SET email = REPLACE(email, SUBSTRING(email, LOCATE('@', email) +1), CONCAT(UUID(), '.com'));
UPDATE email_contact SET email = REPLACE(email, SUBSTRING(email, LOCATE('@', email) +1), CONCAT(UUID(), '.com'));
UPDATE sales_invoice_grid SET customer_email = 'customer@example.com', customer_name  = 'Jack Smith';
UPDATE sales_order SET customer_email = 'customer@example.com', customer_firstname = 'Sally', customer_lastname = 'Smith', remote_ip = '127.0.0.1';
UPDATE sales_order_address SET region = 'Ohio', postcode = '12345-1234', lastname = 'Smith', street = '123 Main street', region_id = 44, city = 'Phoenix', telephone = NULL, firstname = 'Jane', company = NULL;
UPDATE sales_order_grid SET customer_email = 'customer@example.com', shipping_name = 'Jack', billing_name = 'Jack Smith', billing_address = '123 Main Street', shipping_address = '321 Pine Street', customer_name = 'Jane Smith';
UPDATE sales_shipment_grid SET customer_email = 'customer@example.com', customer_name = 'Jane Smith', billing_address = '123 Main street', billing_name = 'Jack Doe', shipping_name = 'Susie Smith';
UPDATE quote SET customer_email = 'customer@example.com', customer_firstname = 'Sally', customer_lastname = 'Jones', customer_dob = NULL, remote_ip = '127.0.0.1';
UPDATE quote_address SET email = 'customer@example.com', firstname = 'Jack', lastname = 'Smith', company = NULL, street = '123 Main st', city = 'AnyCity', region = 'Some State', region_id = 44, postcode = '12345-1234', telephone = NULL;
UPDATE magento_rma SET customer_custom_email = 'customer@example.com' WHERE customer_custom_email IS NOT NULL;
UPDATE customer_address_entity SET firstname = 'Jack', lastname = 'Smith', telephone = '909-555-1212', postcode = NULL,  region = NULL, street = '123 Main street', city = 'Anycity', company = NULL;
UPDATE customer_grid_flat SET name = 'Jane Doe', email = 'customer@example.com', dob = NULL, gender = NULL, taxvat = NULL, shipping_full = '', billing_full = '', billing_firstname = 'Jack', billing_lastname = 'Smith', billing_telephone = NULL, billing_postcode = NULL, billing_country_id = NULL, billing_region = NULL, billing_street = '123 Main street', billing_city = 'Anycity', billing_fax = NULL, billing_vat_id = NULL, billing_company = NULL;
UPDATE sales_creditmemo_grid SET billing_name = 'Sally', billing_address = '123 Main Street', customer_name = 'Jack Smith', customer_email = 'customer@example.com';
UPDATE magento_rma_grid SET customer_name = 'Jack Smith';
UPDATE newsletter_subscriber SET subscriber_email = 'customer@example.com';
UPDATE core_config_data SET value = '' WHERE path = 'orderexport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'productexport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'trackingimport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'stockimport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'remarketing/onescript/merchant_id';
UPDATE core_config_data SET value = '' WHERE path = 'remarketing/onescript/merchant_id';
UPDATE core_config_data SET value = '' WHERE path = 'algoliasearch_credentials/credentials/application_id';
UPDATE core_config_data SET value = '' WHERE path = 'algoliasearch_credentials/credentials/search_only_api_key';
UPDATE core_config_data SET value = '' WHERE path = 'tax/avatax/production_account_number';
UPDATE core_config_data SET value = '' WHERE path = 'tax/avatax/production_license_key';
UPDATE core_config_data SET value = '' WHERE path = 'design/head/includes';
UPDATE core_config_data SET value = '' WHERE path = 'payment/braintree/merchant_id';
UPDATE core_config_data SET value = '' WHERE path = 'payment/braintree/public_key';     
UPDATE core_config_data SET value = '' WHERE path = 'payment/braintree/private_key';
UPDATE core_config_data SET value = '' WHERE path = 'system/full_page_cache/fastly/fastly_service_id';
UPDATE core_config_data SET value = '' WHERE path = 'system/full_page_cache/fastly/fastly_api_key';
UPDATE core_config_data SET value = '' WHERE path = 'google/analytics/container_id';  
UPDATE core_config_data SET value = '' WHERE path = 'analytics/general/token';
UPDATE vault_payment_token SET public_hash = UUID(), details = '{"type":"VI","maskedCC":"1111","expirationDate":"01\/2019"}';
TRUNCATE customer_log; 
TRUNCATE customer_visitor; 
TRUNCATE magento_logging_event;
TRUNCATE oauth_consumer;
TRUNCATE oauth_nonce;
TRUNCATE oauth_token;
TRUNCATE password_reset_request_event;
TRUNCATE acknowledgement;
TRUNCATE acknowledgement_report;
TRUNCATE avatax_log;
TRUNCATE avatax_queue;
TRUNCATE cron_schedule;
SET FOREIGN_KEY_CHECKS=1;
```

また、情報をマスクする代わりにレコードを削除して、新しい DB を小さくすることもできます。 PII がマスクまたは削除されると、データをチームメイトに安全に提供して、ローカル環境で使用できます。

## Adobe Commerce Cloudプロジェクトへのリモート DB 接続

この方法を使用すると、実際のデータを誤って編集および削除することができます。 この方法は、慎重に使用する必要があります。 データベースのバックアップとオフラインでのデータの確認が、推奨されるアプローチです。 Adobe Commerce Cloud上で直接データにアクセスする必要がある場合もありますが、これにはリスクが伴います。 「本当に？」という言葉はありません 質問がある場合は、誤ってデータを変更または削除する可能性があります。

超重要！ リモート DB 接続を行うと便利で、実際のライブデータを使用しますが、リスクが伴います。 私は個人的に、Adobe Commerceの主要なテクニカルアーキテクトとして、それをお勧めしません。 リモート DB 上にいることを忘れて、誤ってデータを削除または変更するのは簡単です。 読み取り専用レプリカに接続するオプションもありますが、SQL アクティビティの重さに応じて、サイトに多少の影響を与えます。 ただし、可能なので、それを達成する手順は次のとおりです。

SSH トンネルを確立します。

```bash
magento-cloud tunnel:open
```

プロジェクトを選択し、環境を選択した後、mysql グラフィカルインターフェイスの設定で使用されるコマンドからの出力が表示されます。

```bash
magento-cloud tunnel:open

Enter a number to choose a project:
  [0] demo-ralbin (ral32nryq4123)
  [1] adobe-commerce-demo (abc123zzkipexnqo)
  [2] DX Tutorials - Commerce (abasrpikfw4123)
 > 2

Enter a number to choose an environment:
Default: master
  [0] master (type: production)
  [1] remote-db (type: development)
 > 1

SSH tunnel opened to database at: mysql://user:@127.0.0.1:30000/main
SSH tunnel opened to redis at: redis://127.0.0.1:30001
SSH tunnel opened to opensearch at: http://127.0.0.1:30002
SSH tunnel opened to rabbitmq at: amqp://guest:guest@127.0.0.1:30003

Logs are written to: /Users/<user>/.magento-cloud/tunnels.log

List tunnels with: magento-cloud tunnels
View tunnel details with: magento-cloud tunnel:info
Close tunnels with: magento-cloud tunnel:close

Save encoded tunnel details to the MAGENTO_CLOUD_RELATIONSHIPS variable using:
  export MAGENTO_CLOUD_RELATIONSHIPS="$(magento-cloud tunnel:info --encode)"
```

MySQL グラフィカルインターフェイスを使用した接続の確立 `SSH tunnel opened to database at` コマンドオプション。

```bash
SSH tunnel opened to database at: mysql://user:@127.0.0.1:30000/main
```

適切な情報が得られたので、引き続きこれらの値を Cloud Console に挿入します。

SSH ホスト名とユーザー名は、Cloud Console のクラウド資格情報から確認できます。

![ロゴ — Adobe Commerce Cloud Console](./assets/cloud-ui-screenshot.png "Adobe Commerce Cloud Console")

次に例を示します。 `ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud`
SSH ホスト名は@記号の後のすべてです。 `ssh.us-4.magento.cloud` この例では、
SSH ユーザー名は@記号の前のすべてです。  `abasrpikfw4123-remote-db-ecpefky—mymagento`

## データベースに接続する値の検索

MariaDB データベースに直接アクセスするには、SSH を使用してリモートクラウド環境にログインし、データベースに接続する必要があります。

1. SSH を使用してリモート環境にログインします。

   ```bash
   magento-cloud ssh
   ```

1. から MySQL ログイン資格情報を取得します。 `database` および `type` プロパティ [$MAGENTO_CLOUD_RELATIONSHIPS](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/app/properties/properties.html?lang=en#relationships) 変数を使用します。

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   または

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   応答で、MySQL 情報を見つけます。 例：

   ```json
   "database" : [
      {
         "password" : "",
         "rel" : "mysql",
         "hostname" : "nnnnnnnn.mysql.service._.magentosite.cloud",
         "service" : "mysql",
         "host" : "database.internal",
         "ip" : "###.###.###.###",
         "port" : 3306,
         "path" : "main",
         "cluster" : "projectid-integration-id",
         "query" : {
            "is_master" : true
         },
         "type" : "mysql:10.3",
         "username" : "user",
         "scheme" : "mysql"
      }
   ],
   ```

次に、MySQL GUI で設定値を使用します。 次の例では MySQL Workbench を使用していますが、MySQL 接続をサポートするアプリにも、同様のフィールドがあります。

![logo - Mysql Workbench を使用した Mysql GUI の例](./assets/mysql-workbench-after-connecting.png " Mysql Workbench を使用した Mysql GUI の例")

![logo - TablesPlus を使用した Mysql GUI の例](./assets/tablesPlus-db-connection.png " TablesPlus を使用した Mysql GUI の例")

すべての設定が完了したら、MySQL GUI を使用して、リモートAdobe Commerce Cloudプロジェクトに対するクエリを実行できます。

## SQL を実行するためのクラウドプロジェクトデータベースへの直接接続

次のメソッドでは、 `magento-cloud` cli を使用して mysql データベースに直接接続し、SQL を実行します。これにより、データベースのクエリを高速化できます。 このデータベースをコピーする必要がある場合は、次の代替方法の 1 つを参照してください。 [データベースダンプの作成](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html).

```bash
magento-cloud db:sql    

Enter a number to choose a project:
  [0] demo-ralbin (ral32nryq4123)
  [1] adobe-commerce-demo (abc123zzkipexnqo)
  [2] DX Tutorials - Commerce (abasrpikfw4123)
 > 2

Enter a number to choose an environment:
Default: master
  [0] master (type: production)
  [1] remote-db (type: development)
 > 1

Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 273454
Server version: 10.6.15-MariaDB-1:10.6.15+maria~deb10-log mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

例えば、 `core_config_data` 単語を含むテーブル `secure` 列の一部として `path`:

```sql
MariaDB [main]> SELECT * FROM core_config_data WHERE path LIKE '%secure%' \G;
*************************** 1. row ***************************
 config_id: 5
     scope: default
  scope_id: 0
      path: web/unsecure/base_url
     value: http://remote-db-ecpefky-abasrpikfw4123.us-4.magentosite.cloud/
updated_at: 2024-02-02 18:03:17
*************************** 2. row ***************************
 config_id: 6
     scope: default
  scope_id: 0
      path: web/secure/base_url
     value: https://remote-db-ecpefky-abasrpikfw4123.us-4.magentosite.cloud/
updated_at: 2024-02-02 18:03:17
*************************** 3. row ***************************
 config_id: 8
     scope: default
  scope_id: 0
      path: web/secure/use_in_adminhtml
     value: 1
updated_at: 2023-04-26 19:43:58
3 rows in set (0.001 sec)

ERROR: No query specified

MariaDB [main]> 
```

## その他のリソース

[Adobe Commerce Cloud CLI](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/dev-tools/cloud-cli/cloud-cli-overview.html)
[MySQL サービスを設定する](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/mysql.html)
[リモート MySQL データベース接続を設定する](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/database-server/mysql-remote.html)
[クラウドインフラストラクチャ上のAdobe Commerceにデータベースダンプを作成する](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html)

---
title: Adobe Commerce データベースに対するクエリの接続と実行
description: Adobe Commerce on cloud プロジェクトに接続し、オフサイトで使用するためのデータベースダンプを作成し、PIIをマスクまたは削除して、Cloud CLI、GUI クライアント、またはダイレクト接続を使用してSQLを実行します。
feature: Backend Development,Console,Cloud
topic: Commerce,Development
role: Developer
level: Intermediate, Experienced
doc-type: Technical Video
duration: 1024
last-substantial-update: 2024-06-25T00:00:00Z
jira: KT-14910
exl-id: e740bbd0-5ec7-4272-89cb-0bed776eb149
source-git-commit: d2c01abbc24ec14f6147004bb9a13ebdbfb8b60e
workflow-type: tm+mt
source-wordcount: '1010'
ht-degree: 0%

---

# Adobe Commerce データベースに対するクエリの接続と実行

Adobe Commerce on cloud プロジェクトに接続する方法、オフサイトで使用するためのデータベースダンプを作成する方法、個人を特定できる情報（PII）をマスクまたは削除する方法について説明します。 ローカル ダンプ、MySQL WorkbenchやTablePlusなどのGUI、または`magento-cloud` CLIを使用してデータにアクセスします。

## 動画コンテンツ

* MySQL WorkbenchやTablePlusなどのGUI ツールを使用して、リモートのAdobe Commerce Cloud プロジェクトに接続します。
* プロジェクトに接続し、コマンドラインからSQLを実行します。

>[!VIDEO](https://video.tv.adobe.com/v/3450037?captions=jpn&learn=on)

クラウドプロジェクトからAdobe Commerce データにアクセスするには、次のいずれかの方法を使用します。

* ローカル DB ダンプを使用します。
* MySQL WorkbenchやTablePlusなどのアプリケーションを使用して、リモートクラウド環境へのDB接続を開きます。
* `magento-cloud` CLIを使用してクラウド環境に直接接続し、リモートサーバーでコマンドを実行します。

顧客情報を削除するためにスクラブするデータベースダンプを好みます。 顧客データは必要ないときに完全に削除します。

## Adobe Commerce Cloud CLI ツールの使用

データベースダンプを作成するには、[Adobe Commerce Cloud CLI](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/dev-tools/cloud-cli/cloud-cli-overview.html?lang=ja)がインストールされている必要があります。 ローカルコンピューターでディレクトリを開き、次のコマンドを実行します。 `your-project-id`をプロジェクト IDに置き換えます（`asasdasd45q`と同様）。 `your-environment-name`を環境名（`master`や`staging`など）に置き換えます。

`magento-cloud db:dump -p your-project-id -e your-environment-name`

プロジェクト IDまたは環境がわからない場合は、次のコマンドで省略できます。

`magento-cloud db:dump`

CLIでは、正しいプロジェクトと環境を指定するように求められます。 次の例は、そのダイアログを表示します。 この例では、アカウントに割り当てられている複数のプロジェクトを示していますが、使用可能なプロジェクトは1つだけである可能性があります。

ディレクトリに変更する

```bash
cd ~/Downloads/db-tutorial 
```

次に、コマンドを実行してデータベースダンプを作成します

```bash
magento-cloud db:dump
```

プロジェクトまたは環境を指定しなかったため、Adobe Commerce Cloud CLIは、いくつかの質問を行います。 次の例は、サンプルダイアログを示しています。

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

## Adobe Commerce ECE-toolsの使用

Adobe Commerce CLI ツールがない場合は、`ssh`をプロジェクトに追加して、`ece` コマンド `vendor/bin/ece-tools db-dump`を実行できます。
回答サンプル：

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

`SFTP`または`rsync`を使用して、データベース ダンプをローカル環境にプルします。

次の例では、`rsync`を使用してファイルを`~/Downloads/db-tutorial` フォルダーにプルします。

```bash
rsync -avrp -e ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud:/app/var/dump-main-1707850906.sql.gz ~/Downloads/db-tutorial
```

ターミナルウィンドウはいくつかの情報を出力します。以下に出力の例を示します

```bash
rsync -avrp -e ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud:/app/var/dump-main-1707850906.sql.gz ~/Downloads/db-tutorial
receiving file list ... done
dump-main-1707850906.sql.gz

sent 38 bytes  received 2691041 bytes  358810.53 bytes/sec
total size is 2690241  speedup is 1.00
```

ファイルの内容を表示して、正常にダウンロードされたことを確認します。

```bash
ls -lah
total 29840
drwxr-xr-x    4 <username>  staff   128B Feb 13 13:02 .
drwx------@ 103 <username>   staff   3.2K Feb 13 12:52 ..
-rw-r--r--    1 <username>   staff    11M Feb 13 12:53 abasrpikfw4123--remote-db-ecpefky--mysql--main--dump.sql
-rw-r--r--    1 <username>   staff   2.6M Feb 13 13:01 dump-main-1707850906.sql.gz
```

データを収集したら、顧客データの削除やマスキングを通じて、データをクリーンアップする必要があります。 次のサンプルスクリプトは、開始するのに役立ちます。

この例では、顧客データをランダムな文字列に変換しますが、すべての項目を保持します。 この例には、顧客PIIがサードパーティテーブルとコアテーブルに含まれることを示す追加テーブルがいくつか含まれています。 あらゆるテーブルのデータを注意深く調べ、顧客データをマスクしたり削除したりします。

通常、データベースダンプのマスキングとサニタイズを担当するのは、アーキテクトまたはリード開発者だけです。 専用のサニタイザーを導入することで、未加工データの露出を低減し、コンプライアンス規則や規制に違反する可能性を低減できます。

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

あるいは、情報をマスクする代わりにレコードを削除して、新しいDBを小さくすることもできます。 PIIがマスクまたは削除されると、データをチームメイトに安全に提供し、ローカル環境で使用できるようになります。

## Adobe Commerce Cloud プロジェクトへのリモート DB接続

この方法を使用すると、ライブデータを誤って編集および削除することができます。 慎重に使用してください。 可能な場合は、データベースのバックアップとオフラインのレビューをお勧めします。 場合によっては、Adobe Commerce Cloudから直接データにアクセスする必要がありますが、そのワークフローには依然としてリスクがあります。 GUIは確認プロンプトを追加しないため、誤ってデータを変更または削除することができます。

リモートデータベース接続は便利ですがリスクがあります。 本番環境に接続していることを簡単に忘れてしまい、データを削除したり変更したりすることができます。 読み取り専用のレプリカに接続することはできますが、大量のSQLがサイトに影響を与えます。 Adobeでは、書き込み可能なデータベースへの日常的なリモート接続は推奨されていません。以下の手順は、リスクを理解している場合にのみ使用してください。

SSH トンネルを確立します。

```bash
magento-cloud tunnel:open
```

プロジェクトを選択して環境を選択すると、mysql グラフィカルインターフェイスの設定で使用されるコマンドの出力が表示されます。

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

`SSH tunnel opened to database at` コマンドオプションを使用して、MySQL グラフィカルインターフェイスを使用して接続を確立します。

```bash
SSH tunnel opened to database at: mysql://user:@127.0.0.1:30000/main
```

適切な情報が得られたので、Cloud Consoleでこれらの値を入力します。

SSH ホスト名とユーザー名は、Cloud Consoleのクラウド資格情報から確認できます。

![Adobe Commerce Cloud Console](./assets/cloud-ui-screenshot.png "Adobe Commerce Cloud Console")

次に例を1つ示します：`ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud`
SSH ホスト名は、この例の@記号`ssh.us-4.magento.cloud`の後のすべてです。
SSH ユーザー名は@記号の前のすべて：`abasrpikfw4123-remote-db-ecpefky--mymagento`

## データベースに接続する値の検索

MariaDB データベースに直接アクセスするには、SSHを使用してリモートクラウド環境にログインし、データベースに接続します。

1. SSHを使用してリモート環境にログインします。

   ```bash
   magento-cloud ssh
   ```

2. `database`$MAGENTO_CLOUD_RELATIONSHIPS`type`変数の[および](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/app/properties/properties.html?lang=ja#relationships) プロパティからMySQL ログイン資格情報を取得します。

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   または

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   応答で、MySQL情報を見つけます。 例：

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

次に、MySQL GUIで設定値を使用します。 次の例では、MySQL Workbenchを使用していますが、MySQL接続をサポートするアプリケーションには同様のフィールドがあります。

![MySQL Workbench接続例](./assets/mysql-workbench-after-connecting.png "MySQL Workbench接続例")

![TablePlus接続例](./assets/tablesPlus-db-connection.png "TablePlus接続例")

接続を設定したら、MySQL GUIを使用して、リモート Adobe Commerce Cloud プロジェクトでクエリを実行できます。

## SQLを実行するためのクラウドプロジェクトデータベースへの直接接続

次のメソッドは、`magento-cloud` CLIを使用してMySQL データベースに直接接続し、SQLを実行してクエリを高速化します。 このデータベースのコピーが必要な場合は、[&#x200B; データベースダンプの作成](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html?lang=ja)に代わりの方法のいずれかを使用します。

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

例えば、列`core_config_data`の一部として単語`secure`を含む`path` テーブルのすべてのレコードを検索できます。

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

## 関連資料

* [Adobe Commerce Cloud CLI](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/dev-tools/cloud-cli/cloud-cli-overview.html?lang=ja)
* [MySQL サービスの設定](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/mysql.html?lang=ja)
* [&#x200B; リモート MySQL データベース接続の設定](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/database-server/mysql-remote.html?lang=ja)
* [&#x200B; クラウドインフラストラクチャ上のAdobe Commerceにデータベースダンプを作成](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html?lang=ja)

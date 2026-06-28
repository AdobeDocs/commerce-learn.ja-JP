---
title: バルクパッケージのグローバル参照アーキテクチャパターン
description: 効率的なコード管理、バージョン管理、スケーラブルなマルチインスタンス展開のために、Bulk Packages GRA パターンを使用してAdobe Commerceを設定する方法を説明します。
jira: KT-16726
doc-type: Tutorial
duration: 296
last-substantial-update: 2025-01-06
feature: Best Practices, Configuration, Install
topic: Architecture, Commerce, Development
badge: label="Contributed by Tony Evers, シニア・テクニカル・アーキテクト，Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony" tooltip="アーティスト：Tony Evers"
role: Developer
level: Beginner, Intermediate
exl-id: ac63e31e-3047-410a-a6f9-a578b495bd8c
TQID: https://experienceleague.adobe.com/q4NzQxc7XJDB-TNv2pU7ghDr6bahliY6soUGPu7fhfg
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: b69b2659-1057-424e-8fc5-ed9e016dc554id: f8a45b24-4be7-4f1b-909b-60d06b483a20id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 776428136218d5d3cf5b1720832798822039aee2
workflow-type: tm+mt
source-wordcount: 1188
ht-degree: 0%

---

# Bulk Packages グローバル参照アーキテクチャパターン

{{only-for-on-prem-commerce-cloud}}

このガイドでは、Bulk Packages Global Reference Architecture （GRA）パターンを使用してAdobe Commerceを設定する方法について説明します。

バルクパッケージ GRA パターンには、すべての一般的なカスタマイズをホストする単一のGit リポジトリが含まれます。 この1つのGit リポジトリは、複数のAdobe Commerce モジュールを含む1つのコンポーザーパッケージとしてComposerを通じて公開されます。

![ コードが一括パッケージ GRA パターンのどこに保存されているかを示す図](/help/assets/global-reference-architecture/bulk-gra-pattern-diagram.png){align="center"}

## このパターンの利点と欠点

利点：

* 共有コードリポジトリによるコードの再利用
* GRAの異なる履歴バージョンを異なるインスタンスにインストールし、段階的なリリースを可能にする柔軟性
* GRAの複数のメジャーバージョンをバックポートおよび維持する柔軟性
* GRAのセマンティック バージョン管理のサポート
* 簡素化：開発者は、通常のシングルストア開発パターンよりも多くのスキルを必要としません
* 特別なツール、複雑なインフラストラクチャ、特別な分岐戦略は必要ありません
* リリース内のパッケージの組み合わせは、常に開発され、テストされます

欠点：

* GRAに含まれるすべてのパッケージを含め、完全なGRAをアップグレードできるのは可能です。
* Adobe Commerce モジュール、言語パックおよびテーマ以外のコンポーザーパッケージのGRA バルクパッケージはサポートされていません。そのため、メタパッケージ、magento2 コンポーネントパッケージ、コンポーザープラグインおよびパッチはサポートされていません

## 分割Git GRA パターンを使用したAdobe Commerceの設定

### ディレクトリ構造

Bulk Packages GRAは、Composer リポジトリを通じて、すべての再利用可能なコードをインストールします。 単一のインスタンスに固有のコードは、そのインスタンスのGit リポジトリ内にあります。 インスタンス固有のコードは、Adobe Commerceの他のインスタンスでは再利用されません。

一括パッケージ GRA パターンを使用したAdobe Commerceの完全インストールの最終的なディレクトリ構造は次のようになります。

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
├── composer.lock
└── packages/
    └── local/
```

Composerはこれらのディレクトリ内のコードを評価しないため、`app/code`、`app/i18n`および`app/design` ディレクトリは意図的に省略されています。 その結果、パッケージで宣言された依存関係は自動的にはインストールされません。 Bulk Packages GRA パターンは、この問題を解決するために、`packages/`にいくつかのカスタムコードをインストールし、そのディレクトリをコンポーザーリポジトリとして扱います。 Composerは、`packages/`内のパッケージを`vendor/`にリンクします。

### Git リポジトリの準備

共有GRA コードと最初のストア用に2つのGit リポジトリを作成します。 次のファイル構造を持つGRA リポジトリから開始します。

その結果、次のディレクトリ構造が得られます。

```text
.
├── composer.json
└── src/
    ├── GraOne/
    │   ├── composer.json
    │   └── registration.php
    ├── GraTwo/
    │   ├── composer.json
    │   └── registration.php
    └── registration.php
```

このディレクトリ構造は、GraOne モジュールとGraTwo モジュールのcomposer.json ファイルとregistration.php ファイルのみを記述します。 実際には、これらのモジュール内にはより多くのファイルがあります。

Git リポジトリを開始するには、次のコマンドを実行します。

```bash
mkdir gra-bulk-foundation
cd gra-bulk-foundation
git init
git remote add origin git@github.com:AntonEvers/gra-bulk-foundation.git
vim composer.json # see code snippet below for contents
git add composer.json
git commit -m 'initialize GRA foundation repository'
git push -u origin main
```

composer.json ファイルの内容：

```json
{
    "name": "antonevers/gra-bulk-foundation",
    "description": "Shared code repository",
    "type": "library",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {},
    "autoload": {
        "files": [
            "src/registration.php"
        ],
        "psr-0": {
            "": "src/"
        }
    }
}
```

composer.json パッケージの名前空間を独自の名前空間に変更します。

src/registration.php ファイルの内容：

```php
<?php

declare(strict_types=1);

$pathList[] = dirname(__DIR__) . '/src/*/*/registration.php';
foreach ($pathList as $path) {
    $files = glob($path, GLOB_NOSORT | GLOB_ERR);
    if ($files === false) {
        throw new RuntimeException('glob() returned error while searching in \'' . $path . '\'');
    }
    foreach ($files as $file) {
        require_once $file;
    }
}
```

registration.php ファイルは、Adobe Commerce モジュール内の他のregistration.php ファイルを探して実行します。

<https://github.com/AntonEvers/gra-bulk-foundation>のコードを使用して、2つのサンプルモジュールを作成します。 Composerでは、サンプルモジュール内のcomposer.json ファイルは評価されません。 彼らは習慣としてそこにいる。 モジュールを別の場所に移動する場合は、composer.json ファイルが再度必要になります。

### ストアリポジトリの設定

デプロイメントリポジトリには、GRA コードを含むAdobe Commerceのインストール全体が含まれます。 デプロイメントリポジトリを作成します。

```bash
mkdir gra-bulk-brand-x
cd gra-bulk-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-bulk-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

`bin/magento setup:install`でAdobe Commerceをインストールします。 Composerを使用して、デプロイメントリポジトリにGRA サンプルモジュールをインストールします。

```bash
composer config repositories.gra-foundation vcs git@github.com:AntonEvers/gra-bulk-foundation.git
composer require antonevers/gra-bulk-foundation:@dev
bin/magento module:enable AntonEvers_GraOne AntonEvers_GraTwo
bin/magento test:gra-one
bin/magento test:gra-two
git add app/etc/config.php composer.json composer.lock
git commit -m 'install GRA foundation'
git push origin main
```

この最後のコマンドは、モジュールがインストールされ、動作していることを証明するために、次の出力になります。

```bash
GRA One module is installed successfully and working!
GRA Two module is installed successfully and working!
```

複数のバルクパッケージを作成して、コードを整理できます。 例えば、Composerでは利用できないサードパーティコード用のサードパーティのバルクパッケージなどです。 従来は`app/code`にインストールしていた項目はすべて、バルクパッケージの`src/` ディレクトリに配置する必要があります。 そのルールの例外は、1つのインスタンスでのみ使用されるコードです。 これらのパッケージはローカルパッケージと呼ばれます。

### ローカルパッケージのインストール

デプロイメントリポジトリは、ローカルパッケージをホストします。 GRA バルクパッケージには存在しません。 ローカルパッケージの場所は`app/code`ではなく`packages/local`です。 このディレクトリをリポジトリとして扱うようにComposerに指示します。

```bash
composer config repositories.local path 'packages/local/*/*'
git add composer.json
git commit -m 'initialize local composer package storage'
git push origin main
```

<https://github.com/AntonEvers/module-example-local>でホストされているモジュールの例を追加します。

```bash
mkdir -p packages/local
cd packages/local
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-local/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-local-main module-local
git add module-local
cd ../../..
composer require antonevers/module-local:@dev
bin/magento module:enable AntonEvers_Local
bin/magento test:local
```

この最後のコマンドは、モジュールがインストールされ、動作していることを証明するために、次の出力になります。

```bash
Local module is installed successfully and working!
```

ローカルモジュールをブランドリポジトリにコミットします。

```bash
git add packages/local/antonevers/module-local app/etc/config.php composer.json composer.lock 
git commit -m 'add local module'
git push origin main
```

## コードの場所の概要

サードパーティがComposer リポジトリを介したインストールを提供しない場合にのみ、サードパーティのモジュールを基盤リポジトリの`src/` ディレクトリまたは専用のサードパーティのバルクパッケージに保存できます。

* **Adobe Commerce core**: repo.magento.comから利用できます。
* **サードパーティ製モジュール**: Marketplaceまたはベンダー独自のComposer リポジトリから入手できます。
* **サードパーティ製モジュールのフォールバックオプション**：一括パッケージの`src/`に保存されます。
* **GRA基盤コード**：基盤バルクパッケージの`src/`に格納されています。
* **ローカルコード**：デプロイメントリポジトリの`packages/local` ディレクトリに保存されます。

## GRA モジュールの開発

ソースからバルクパッケージをインストールして、バルクパッケージディレクトリでGitを有効にします。

```bash
rm -r vendor/antonevers/gra-bulk-foundation
composer install --prefer-source
```

バルクパッケージはGitを使用してチェックアウトされました。 `vendor/antonevers/gra-bulk-foundation` ディレクトリに入ると、Git リポジトリもGa-bulk-foundationに入ります。 このディレクトリでブランチを作成、チェックアウト、結合できます。

GRA バルクパッケージのルートにあるcomposer.json ファイルにComposerの依存関係を追加します。これは、Composerが評価するバルクパッケージ内の唯一のファイルです。

## GRA バルクパッケージにサードパーティ製モジュールを含める

GRA基盤のルートにあるcomposer.jsonのrequire セクションにサードパーティパッケージを追加して、GRAに追加します。 これにより、パッケージは常にcomposerを通じてすべてのインスタンスにインストールされます。

## コードを配信する

メインブランチにコードを配信するには、2つのパスがあります。 まず、メインブランチにマージされるローカルモジュール。 これらのモジュールに対してComposerの更新を実行します。 競合を減らすために、開発者がチケットブランチのcomposer.lockを更新することを許可しないでください。 ステージングブランチと実稼動ブランチのcomposer.lock ファイルのみを更新することで、競合のリスクを軽減します。

次に、GRA バルク パッケージ。GRA バルク リポジトリのメイン ブランチにマージされます。 次に、Composer パッケージのバージョンを作成して、Git タグをメインブランチに追加します。 デプロイメントリポジトリのcomposer.jsonに新しいバージョンのGRA バルクパッケージをインストールする必要があります。

## 分岐戦略

このGRA パターンは、GRA バルクリポジトリ内のデプロイメントリポジトリの分岐戦略をミラーリングしている限り、すべての分岐戦略で機能します。 リリースの場合は、両方のリポジトリに同じ名前のリリースブランチを作成します。 開発の場合は、両方のリポジトリにチケットブランチを作成します。

チケットブランチでは、composer.lock ファイルを更新する必要はほとんどありません。 ストアとGitを使用したGRA基盤リポジトリの両方について、開発環境の適切なブランチを確認してください。 例外は、GRA foundation composer.json ファイルの要件を更新する場合です。 デプロイメントリポジトリ内のGRA基盤のアップグレードは、リリースのビルド時、またはQA ブランチのビルド時にのみ実行されます。

## コード例

この記事のコード例は、概念実証のテストに使用できるGit リポジトリのセットとして使用できます。

* 実稼動ストアの例：<https://github.com/AntonEvers/gra-bulk-brand-x>
* GRA コード リポジトリ：<https://github.com/AntonEvers/gra-bulk-foundation>
* ローカル モジュールの例：<https://github.com/AntonEvers/module-example-local>

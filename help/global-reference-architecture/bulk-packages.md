---
title: バルクパッケージのグローバルリファレンスアーキテクチャを使用したAdobe Commerceの最適化
description: 効率的なコード管理とバージョン管理を実現するために、バルクパッケージのグローバルリファレンスアーキテクチャを使用してAdobe Commerceをセットアップする方法について説明します。
jira: KT-16726
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
topic: Architecture, Commerce, Development
badge: label="執筆：Adobe、シニアテクニカルアーキテクト、Tony Evers" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="寄稿：Tony Evers"
role: Architect, Developer, User, Leader
level: Beginner, Intermediate
exl-id: ac63e31e-3047-410a-a6f9-a578b495bd8c
source-git-commit: a182b97b7d9a8bf114944d5d920afb2328adbc18
workflow-type: tm+mt
source-wordcount: '1172'
ht-degree: 0%

---

# バルクパッケージのグローバル参照アーキテクチャパターン

このガイドでは、バルクパッケージのグローバルリファレンスアーキテクチャ（GRA）パターンを使用してAdobe Commerceをセットアップする方法について説明します。

バルクパッケージの GRA パターンでは、共通のすべてのカスタマイズをホストする単一の Git リポジトリーが関与します。 この 1 つの Git リポジトリーは、複数のAdobe Commerce モジュールを含む 1 つのコンポーザーパッケージとして Composer を通じて公開されます。

![&#x200B; バルクパッケージの GRA パターン内のどこにコードが格納されているかを示す図 &#x200B;](/help/assets/global-reference-architecture/bulk-gra-pattern-diagram.png){align="center"}

## このパターンの長所と短所

メリット：

- 共有コードリポジトリを使用したコードの再利用
- 異なるインスタンスに異なる履歴バージョンの GRA をインストールして、段階的なリリースを可能にする柔軟性
- GRA の複数のメジャーバージョンをバックポートおよび保守する柔軟性
- GRA のセマンティックバージョニングのサポート
- シンプル。開発者は、通常のシングルストア開発パターン以上のスキルを必要としません
- 特別なツール、複雑なインフラストラクチャ、特別な分岐戦略は不要
- リリースでのパッケージの組み合わせは、常に一緒に開発およびテストされます

デメリット：

- GRA に含まれるすべてのパッケージも含め、完全な GRA をアップグレードすることのみ可能です。
- Adobe Commerce モジュール、言語パック、テーマ以外のコンポーザパッケージの GRA 一括パッケージではサポートされないため、メタパッケージ、magento2 コンポーネントパッケージ、Composer プラグイン、パッチはサポートされません

## 分割 Git GRA パターンを使用したAdobe Commerceのセットアップ

### ディレクトリ構造

バルクパッケージ GRA は、Composer リポジトリを通じてすべての再利用可能なコードをインストールします。 単一のインスタンスに固有のコードは、そのインスタンスの Git リポジトリ内に存在します。 インスタンス固有のコードは、Adobe Commerceの他のインスタンスでは再利用されません。

バルクパッケージ GRA パターンを使用した完全なAdobe Commerce インストールの最終的なディレクトリ構造は、次のようになります。

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

`app/code`、`app/i18n`、および `app/design` ディレクトリは、Composer がこれらのディレクトリ内のコードを評価しないため、意図的に省略されます。 その結果、パッケージで宣言された依存関係は自動的にはインストールされません。 バルクパッケージの GRA パターンでは、`packages/` にカスタムコードをインストールし、そのディレクトリをコンポーザリポジトリとして扱うことで、この問題を解決します。 Composer は、`packages/` 内のパッケージを `vendor/` にシンボリックリンクします。

### Git リポジトリの準備

共有 GRA コード用と最初のストア用に、2 つの Git リポジトリを作成します。 まず、次のファイル構造を持つ GRA リポジトリから開始します。

結果として、次のディレクトリ構造が得られます。

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

このディレクトリ構造は、GraOne と GraTwo モジュールの composer.json ファイルと registration.php ファイルのみを記述しています。 実際には、これらのモジュール内により多くのファイルがあります。

次のコマンドを実行して Git リポジトリを開始します。

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

src/registration.php ファイルの内容は次のとおりです。

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

registration.php ファイルは、Adobe Commerce モジュール内で他の registration.php ファイルを探し、それらを実行します。

<https://github.com/AntonEvers/gra-bulk-foundation> のコードを使用して 2 つのサンプルモジュールを作成します。 Composer は、サンプル モジュール内の composer.json ファイルを評価しません。 彼らは習慣としてそこにあります。 モジュールを別の場所に移動する場合は、composer.json ファイルが再び必要になります。

### ストアレポジトリの設定

デプロイメントリポジトリには、GRA コードを含むAdobe Commerce インストール全体が含まれています。 デプロイメントリポジトリを作成します。

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

`bin/magento setup:install` と共にAdobe Commerceをインストールします。 Composer を使用して、GRA サンプル モジュールをデプロイメントリポジトリにインストールします。

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

この最後のコマンドにより、モジュールがインストールされ動作していることを示す次の出力が得られます。

```bash
GRA One module is installed successfully and working!
GRA Two module is installed successfully and working!
```

複数のバルクパッケージを作成してコードを整理できます。 例えば、Composer で使用できないサードパーティ コード用のサードパーティ バルク パッケージなどです。 従来 `app/code` にインストールしていたすべてのものは、これでバルクパッケージの `src/` ディレクトリに配置されるはずです。 そのルールの例外は、1 つのインスタンスでのみ使用されるコードです。 これらのパッケージはローカルパッケージと呼ばれます。

### ローカルパッケージのインストール

デプロイメントリポジトリは、ローカルパッケージをホストします。 それらは GRA バルクパッケージには住んでいません。 ローカルパッケージの場所は `app/code` ではなく、`packages/local` です。 このディレクトリをリポジトリとして扱うように Composer に指示します。

```bash
composer config repositories.local path 'packages/local/*/*'
git add composer.json
git commit -m 'initialize local composer package storage'
git push origin main
```

<https://github.com/AntonEvers/module-example-local> でホストされているサンプルモジュールを追加します。

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

この最後のコマンドにより、モジュールがインストールされ動作していることを示す次の出力が得られます。

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

サードパーティが Composer リポジトリを通じてインストールを提供しない場合にのみ、サードパーティのモジュールを基盤リポジトリの `src/` ディレクトリまたは専用のサードパーティのバルクパッケージに保存できます。

- **Adobe Commerce core**:repo.magento.comから入手できます。
- **サードパーティモジュール**:Marketplace またはベンダー独自の Composer リポジトリから利用できます。
- **サードパーティモジュールのフォールバックオプション**：バルクパッケージの `src/` に保存されます。
- **GRA 基盤コード**：基盤バルクパッケージの `src/` に保存されます。
- **ローカルコード**：デプロイメントリポジトリの `packages/local` ディレクトリに保存されます。

## GRA モジュールの開発

ソースからバルクパッケージをインストールして、バルクパッケージディレクトリで Git を有効にします。

```bash
rm -r vendor/antonevers/gra-bulk-foundation
composer install --prefer-source
```

バルクパッケージは Git を使用してチェックアウトされています。 `vendor/antonevers/gra-bulk-foundation` ディレクトリに入ると、gra-bulk-foundation Git リポジトリにも入ることになります。 このディレクトリでブランチの作成、チェックアウトおよびマージを行うことができます。

Composer が評価するバルク パッケージ内の唯一のファイルである GRA バルク パッケージのルートにある composer.json ファイルに Composer 依存関係を追加します。

## GRA バルクパッケージにサードパーティモジュールを含める

GRA 基盤のルートにある composer.json の require セクションにサードパーティパッケージを追加し、GRA に追加します。 これにより、コンポーザーを通じてすべてのインスタンスにパッケージが常にインストールされます。

## コードの配信

main ブランチにコードを配信するには、2 つのパスがあります。 まず、main ブランチにマージされるローカルモジュール。 これらのモジュールに対して Composer アップデートを実行します。 競合を減らすために、開発者がチケットのブランチで composer.lock を更新できないようにします。 ステージングおよび実稼動のブランチの composer.lock ファイルのみを更新して、競合のリスクを軽減します。

次に、GRA バルクリポジトリのメインブランチにマージされる GRA バルクパッケージ。 その後、Git タグを main ブランチに追加し、Composer パッケージのバージョンを管理します。 インストールするには、デプロイメントリポジトリの composer.json に新しいバージョンの GRA バルクパッケージが必要です。

## 分岐戦略

GRA バルクリポジトリ内のデプロイメントリポジトリのブランチ戦略を反映している限り、この GRA パターンはすべてのブランチ戦略で機能します。 リリースの場合、両方のリポジトリで同じ名前のリリースブランチを作成します。 開発のために、両方のリポジトリにチケットブランチを作成します。

チケット分岐では、composer.lock ファイルを更新する必要はほとんどありません。 Git を使用して、ストアと GRA 基盤リポジトリの両方について、開発環境の適切なブランチを確認するだけです。 ただし、GRA 基盤の composer.json ファイルの要件を更新する場合は例外です。 デプロイメントリポジトリ内の GRA 基盤のアップグレードは、リリースを構築する場合、または QA ブランチを構築する場合にのみ行われます。

## コードの例

この記事のコード例は、概念実証のテストに使用できる一連の Git リポジトリとして提供されています。

- 実稼動ストアの例：<https://github.com/AntonEvers/gra-bulk-brand-x>
- GRA コードリポジトリ：<https://github.com/AntonEvers/gra-bulk-foundation>
- ローカルモジュールの例：<https://github.com/AntonEvers/module-example-local>

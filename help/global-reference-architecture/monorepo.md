---
title: グローバルリファレンスアーキテクチャ Monorepo
description: グローバルな参照アーキテクチャに monorepo アプローチを使用して、拡張性と回復力のあるコマースエクスペリエンスを確立する方法を説明します
jira: KT-16728
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="執筆：Adobe、シニアテクニカルアーキテクト、Tony Evers" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="寄稿：Tony Evers"
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer, User, Leader
level: Experienced
exl-id: ebdc13cf-c452-4728-af00-c3ea1149c2fa
source-git-commit: 79d57d2c04c42a8dc23b5735e72e841b7e51cc63
workflow-type: tm+mt
source-wordcount: '1371'
ht-degree: 0%

---

# Monorepo グローバル参照アーキテクチャパターン

{{only-for-on-prem-commerce-cloud}}

このガイドでは、Monorepo グローバルリファレンスアーキテクチャ（GRA）パターンを使用してAdobe Commerceをセットアップする方法について説明します。

Monorepo GRA パターンでは、すべての一般的なカスタマイズをホストする単一の Git リポジトリーが関与します。 この単一の Git リポジトリは、個別の Composer パッケージとして Composer を通じて公開されます。

![ モノレポ GRA パターンのどこにコードが格納されているかを示す図 ](/help/assets/global-reference-architecture/monorepo-gra-pattern-diagram.png){align="center"}

## このパターンの長所と短所

メリット：

- 機能テストに最適
- 共有コードリポジトリを使用したコードの再利用
- パッケージのインストールの柔軟性が高く、各 GRA パッケージは個別にアップグレード、ダウングレード、またはバックポートできます
- セマンティックバージョニングのフルサポート
- 特別なツール、複雑なインフラストラクチャ、特別な分岐戦略は不要
- Composer がサポートするすべてのパッケージ タイプのサポート
- 一時的な環境に最適です。これはオプションですが、大量の配信チームには非常に役立ちます

デメリット：

- 同じ設定で開発されていないパッケージの組み合わせをデプロイすることは可能で、厳密なテスト手順が必要です
- モノレポの GRA パターンは、最初に複雑になる場合があります。 チームがシステムを操作するのに役立つリードを割り当てます

## 別個のパッケージ GRA パターンを使用したAdobe Commerceのセットアップ

### ディレクトリ構造

別個のパッケージ GRA パターンを持つ完全なAdobe Commerce インストールの最終的なディレクトリ構造は、次のディレクトリ構造となります。

```text
.
├── app/
│   └── etc/
│       └── config.php
├── packages/
│   └── ...
├── composer.json
└── composer.lock
```

実稼動 Git リポジトリは、次のディレクトリ構造を持っています。

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
└── composer.lock
```

違いは、実稼動インスタンスは Composer からインストールされ、monorepo はパッケージディレクトリ内のすべてのパッケージの独自のコピーを保持する点です。

### 実稼動リポジトリの準備

1 つ目のAdobe Commerce インスタンス用のリポジトリーを作成します。これは、Brand X の web ストアを表します。

```bash
mkdir gra-monorepo-brand-x
cd gra-monorepo-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-monorepo-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

`bin/magento setup:install` と共にAdobe Commerceをインストールします。 生成された `app/etc/config.php` と composer ファイルをコミットします。 Composer は他のものを管理するので、Git には他に何も含めないでください。

### monorepo リポジトリの準備

monorepo リポジトリも同じ手順で開始します。

```bash
mkdir gra-monorepo 
cd gra-monorepo
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
```

`bin/magento setup:install` を使用してAdobe Commerceをインストールし、コミットしてプッシュします。

```bash
git init
git remote add origin git@github.com:AntonEvers/gra-monorepo.git 
git add composer.json composer.lock app/etc/config.php
git commit -m 'initialize monorepo GRA development repository'
git push -u origin main
```

### モノレポ開発の準備

モノリポジトリ開発の場合、composer.json ファイルに次の変更を加えます。

1. このパッケージが monorepo パッケージであることが明確になるように、パッケージの名前と説明を変更します。
1. バージョンはこのリポジトリの Git タグを使用して管理されるので、composer.json から version 属性を削除します。
1. require セクションを、後で作成されるメタパッケージに置き換えます。
1. 最小安定性を開発に変更します。
1. 含む各パッケージのブランチエイリアスを含め、monorepo パッケージをホストする `packages/*/*` を指すパスタイプリポジトリを追加します
1. プロジェクト自体の分岐エイリアスを追加します

次の Git の差分は、Adobe Commerceのクリーンなインストールと、前述の変更内容の違いを示しています。

```diff
@@ -1,6 +1,6 @@
 {
-    "name": "magento/project-enterprise-edition",
-    "description": "eCommerce Platform for Growth (Enterprise Edition)",
+    "name": "antonevers/gra-monorepo",
+    "description": "Monorepo repository for Global Reference Architecture development",
     "type": "project",
     "license": [
         "proprietary"
@@ -15,11 +15,8 @@
         "preferred-install": "dist",
         "sort-packages": true
     },
-    "version": "2.4.7-p3",
     "require": {
-        "magento/product-enterprise-edition": "2.4.7-p3",
-        "magento/composer-dependency-version-audit-plugin": "~0.1",
-        "magento/composer-root-update-plugin": "^2.0.4"
+        "antonevers/gra-meta-brand-x": "self.version"
     },
     "autoload": {
         "exclude-from-classmap": [
@@ -69,16 +66,33 @@
             "Magento\\Tools\\Sanity\\": "dev/build/publication/sanity/Magento/Tools/Sanity/"
         }
     },
-    "minimum-stability": "stable",
+    "minimum-stability": "dev",
     "prefer-stable": true,
     "repositories": [
         {
+            "type": "path",
+            "url": "packages/*/*",
+            "options": {
+                "versions": {
+                    "antonevers/gra-meta-brand-x": "1.4.x-dev",
+                    "antonevers/gra-meta-foundation": "1.4.x-dev",
+                    "antonevers/gra-component-foundation": "1.4.x-dev",
+                    "antonevers/module-gra": "1.4.x-dev",
+                    "antonevers/module-3rdparty": "1.4.x-dev",
+                    "antonevers/module-local": "1.4.x-dev"
+                }
+            }
+        },
+        {
             "type": "composer",
             "url": "https://repo.magento.com/"
         }
     ],
     "extra": {
-        "magento-force": "override"
+        "magento-force": "override",
+        "branch-alias": {
+            "dev-main": "1.4.x-dev"
+        }
     }
 }
```

### メタパッケージの使用

GitHub の [AntonEvers/gra-meta-foundation](https://github.com/AntonEvers/gra-meta-foundation) からサンプルコードをダウンロードして、この例で使用されているメタパッケージとサンプルモジュールを取得します。

Composer メタパッケージは、複数の composer パッケージを 1 つのパッケージにまとめたものです。 メタパッケージが必要な場合、そのバンドルが Composer を介して自動的にインストールされるすべてのパッケージには、メタパッケージのセクションが必要です。

この例では、次の 2 つのメタパッケージがあります。

1. **antonevers/gra-meta-brand-x**:「ブランド X」を構成するすべてを含むメタパッケージ
1. **antonevers/gra-meta-foundation**：どのブランドにも常にインストールされているすべてのものを含んだメタパッケージ

ブランドメタパッケージには、基盤メタパッケージが必要です。 ブランドメタパッケージが必要な場合は、自動的に基盤メタパッケージも必要になります。 メタパッケージの 2 つの composer.json ファイルを参照して、それらが関連していることを確認してください。

antonevers/gra-meta-brand-x:

```json
{
    "name": "antonevers/gra-meta-brand-x",
    "type": "metapackage",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {
        "antonevers/gra-meta-foundation": "^1.4",
        "antonevers/module-local": "^1.4"
    }
}
```

antonevers/gra-meta-foundation:

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {
        "antonevers/gra-component-foundation": "^1.4",
        "antonevers/module-gra": "^1.4",
        "antonevers/module-3rdparty": "^1.4",
        "magento/composer-dependency-version-audit-plugin": "~0.1",
        "magento/composer-root-update-plugin": "^2.0.4",
        "magento/product-enterprise-edition": "2.4.7-p3"
    }
}
```

メタパッケージは、一緒に属するコードを整理するのに適した方法です。 メタパッケージを使用して、地域、グローバル、ブランド固有のパッケージ、または意味のあるグループ化のパッケージのグループを定義します。 Adobe Commerceの複数のインストールを管理する場合、は、パッケージの想定されるコンテキストを定義する安全で汎用性の高い方法をパッケージ化します。

メタパッケージは `packages` ディレクトリ内の monorepo に存在します。 そこで、`vendor` のディレクトリ構造は次のようにミラーリングされます。

```text
.
├── packages/
│   └── antonevers
│       ├── gra-meta-brand-x
│       │   └── composer.json
│       └── gra-meta-foundation
│           └── composer.json
├── composer.json
└── composer.lock
```

### モジュールの追加と開発

monorepo のモジュールは、`packages` ディレクトリに存在します。 このようにして、Composer はパス タイプ リポジトリを介してそれらを見つけることができます。

GitHub の [AntonEvers/gra-meta-foundation](https://github.com/AntonEvers/gra-meta-foundation) からサンプルコードをダウンロードして、この例で使用されているメタパッケージとサンプルモジュールを取得します。

```text
.
├── packages/
│   └── antonevers
│       ├── gra-meta-brand-x
│       ├── gra-meta-foundation
│       ├── module-3rdparty
│       ├── module-gra
│       └── module-local
├── composer.json
└── composer.lock
```

必要に応じて、`packages` ディレクトリ内に複数の名前空間を含めることができます。

開発は、パッケージディレクトリで行われます。 `packages` ディレクトリ内のパッケージへのシンボリックリンクは、実 `vendor` 動後に `composer update` ディレクトリに作成されます。 これにより、コードはAdobe Commerce インストールの一部になります。

特定のモジュールに対してのみ `bin/magento module:enable --all` またはを実行し、追加されたモジュールを有効にします。

これで、3 つのサンプルモジュールがインストールされ動作する、Adobe Commerceのインストールが機能するようになります。 モジュールがインストールされ、機能しているかどうかを検証するには、次のコマンドを実行します。

```bash
bin/magento test:gra
bin/magento test:3rdparty
bin/magento test:local
```

### 自動パッケージ作成の実現

自動パッケージ作成を実行するには、複数のオプションがあります。 次のようなオプションがあります。

1. [ プライベートパッケージスト ](https://packagist.com/)
1. [Simplyfy Monorepo Builder](https://github.com/symplify/monorepo-builder)
1. 独自のソリューションの構築

[Private Packagist](https://packagist.com/) は、Git monorepo 内のパッケージの認識を自動化し、Composer を通じて公開します。 Adobe Commerceと互換性があり、迅速でメンテナンスが少なく、エラーが発生しやすいので、このガイドではプライベートパッケージストオプションに重点を置いています。

プライベートパッケージストの設定方法については、このガイドの範囲外です。[ ドキュメント ](https://packagist.com/docs) を参照してください。

組織の同期を設定し、Git リポジトリがプライベートパッケージストと自動的に同期されると、パッケージをモノリポジトリに変換する可能性があります。

まず、「パッケージ」タブに移動し、monorepo を見つけます。

![ パッケージ画面に表示される monorepo パッケージを含んだプライベートパッケージストのスクリーンショット ](/help/assets/global-reference-architecture/packagist-packages-before-multi-package.png){align="center"}

monorepo パッケージをクリックし、詳細画面の「編集」をクリックすると、次のページが表示されます。

![monorepo パッケージ編集ページを使用した Private Packagist のスクリーンショット ](/help/assets/global-reference-architecture/packagist-packages-edit.png)

最初の入力フィールドの下に、「マルチパッケージリポジトリの作成」というリンクがあります。 このリンクをクリックします。

![ マルチパッケージ設定を使用したプライベートパッケージストのスクリーンショット ](/help/assets/global-reference-architecture/packagist-packages-multi-package.png)

monorepo 内の composer パッケージの場所を定義します。 この例では、場所は `packages/**/composer.json` です。 プライベートパッケージストが抽出するパッケージを検索する場所を制限または拡張するように場所を変更します。

「パッケージ」タブには、保存後に見つかったすべてのパッケージが表示され、monorepo 自体は Composer パッケージとして表示されなくなります。

![ パッケージ画面に表示されるすべての monorepo パッケージを含むプライベートパッケージストのスクリーンショット ](/help/assets/global-reference-architecture/packagist-packages-after-multi-package.png)

バージョンは、Git の monorepo で作成されたすべてのタグまたはブランチについて、monorepo 内のパッケージごとに Composer で作成されます。

## パッケージを実稼動環境にインストールします

Private Packagist の指示に従って、Private Packagist をコンポーザリポジトリとして追加します。 プライベートパッケージストは、すべての Composer リポジトリおよび Git リポジトリ（packagist.orgなど）のミラーとして使用でき、またそうする必要があります。 この方法では、資格情報を開発者と共有する必要はなく、各パッケージを完全に制御できます。 この例は、Adobe Commerce コードベースを公開する可能性があるので、このベストプラクティスには従いません。

実稼働ストアの例を見るには、GitHub から [GRA Monorepo Brand X](https://github.com/AntonEvers/gra-monorepo-brand-x) をダウンロードしてください。

実稼動ストアには `packages` ディレクトリがなく、すべてのパッケージは Composer を使用してインストールされます。 必要な唯一のパッケージは次のとおりです。

```json
    "require": {
        "antonevers/gra-meta-brand-x": "^1.0"
    },
```

しかし、Adobe Commerce全体と GRA 全体は、このメタパッケージの要件を通じてインストールされます。

## バージョン管理

monorepo 内のすべてのパッケージは、monorepo 自体と同じバージョンを受け取ります。 アプリケーション全体の新しいバージョンを公開する場合と考えてください。 ただし、実稼動環境では、異なる monorepo バージョンから複数のパッケージをインストールできます。

## 一時的な環境

一時的環境を使用する場合や、使用を予定している場合は、モノレポが最適です。 monorepo のすべてのバージョンとブランチには、すべてのAdobe Commerce、サードパーティ、カスタムモジュールファイルが含まれています。 すべてのブランチで完全にインストールすると、機能テストを含むすべてのタイプのテストを実行できます。 個別のパッケージやバルクパッケージ GRA などの他の GRA 設定では、機能テストを実行する前に、まず動作するAdobe Commerce環境を構築する必要があります。 DevOps の観点からは、monorepo を使用すると、はるかに簡単になります。

## コードの例

この記事のコード例は、一連の Git リポジトリーに組み合わされており、概念実証で遊ぶために使用できます。

- monorepo リポジトリの例：<https://github.com/AntonEvers/gra-monorepo>
- 実稼動ストアの例：<https://github.com/AntonEvers/gra-monorepo-brand-x>

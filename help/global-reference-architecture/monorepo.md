---
title: Monorepoのグローバル参照アーキテクチャ
description: Monorepoを活用してグローバル参照アーキテクチャを構築し、拡張性と回復力の高いコマース体験を実現する方法をご確認ください
jira: KT-16728
doc-type: tutorial
duration: 441
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="Contributed by Tony Evers, シニア・テクニカル・アーキテクト，Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="アーティスト：Tony Evers"
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer, User, Leader
level: Experienced
exl-id: ebdc13cf-c452-4728-af00-c3ea1149c2fa
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '1371'
ht-degree: 0%

---

# Monorepo グローバル参照アーキテクチャパターン

{{only-for-on-prem-commerce-cloud}}

このガイドでは、Monorepo Global Reference Architecture （GRA）パターンを使用してAdobe Commerceを設定する方法について説明します。

Monorepo GRA パターンには、すべての一般的なカスタマイズをホストする単一のGit リポジトリが含まれます。 この単一のGit リポジトリは、Composerを通じて個別のコンポーザーパッケージとして公開されます。

![ モノレポ GRA パターン内のコードの保存場所を示す図](/help/assets/global-reference-architecture/monorepo-gra-pattern-diagram.png){align="center"}

## このパターンの利点と欠点

利点：

* 機能テストに最適
* 共有コードリポジトリによるコードの再利用
* パッケージインストールの完全な柔軟性により、各GRA パッケージを個別にアップグレード、ダウングレード、またはバックポートできます
* セマンティックバージョン管理の完全サポート
* 特別なツール、複雑なインフラストラクチャ、特別な分岐戦略は必要ありません
* Composerがサポートするすべてのパッケージタイプのサポート
* オプションの一時的な環境に最適ですが、大量の配信チームには非常に便利です

欠点：

* 同じ構成で開発されていないパッケージの組み合わせをデプロイすることが可能で、厳格なテスト手順が必要です
* モノレポ GRA パターンは、最初は複雑になる可能性があります。 チームがシステムを利用するのに役立つリードを割り当てる

## Adobe Commerceを個別のパッケージ GRA パターンで設定する

### ディレクトリ構造

パッケージの分離GRA パターンを持つ完全なAdobe Commerce インストールの最終的なディレクトリ構造には、次のディレクトリ構造があります。

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

実稼動Git リポジトリには、次のディレクトリ構造があります。

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
└── composer.lock
```

違いは、実稼動インスタンスがComposerからインストールされる点です。この場合、monorepoはパッケージディレクトリ内のすべてのパッケージの独自のコピーを保持します。

### 実稼動リポジトリの準備

Brand Xのweb ストアを表す最初のAdobe Commerce インスタンスのリポジトリを作成します。

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

`bin/magento setup:install`でAdobe Commerceをインストールします。 結果の`app/etc/config.php`とコンポーザーファイルをコミットします。 Composerは他の何かを管理するので、Gitに他に何も入れてはなりません。

### monorepo リポジトリの準備

monorepo リポジトリは同じ手順で始まります。

```bash
mkdir gra-monorepo 
cd gra-monorepo
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
```

`bin/magento setup:install`でAdobe Commerceをインストールし、コミットしてプッシュします。

```bash
git init
git remote add origin git@github.com:AntonEvers/gra-monorepo.git 
git add composer.json composer.lock app/etc/config.php
git commit -m 'initialize monorepo GRA development repository'
git push -u origin main
```

### モノレポ開発の準備

monorepo開発の場合は、composer.json ファイルに次の変更を加えます。

1. パッケージの名前と説明を変更して、このパッケージがmonorepo パッケージであることを明確にします。
1. バージョンは、このリポジトリのGit タグを使用して管理されるので、composer.jsonからバージョン属性を削除します。
1. 「要求」セクションを、後で作成するメタパッケージに置き換えます。
1. 最小安定性を開発に変更します。
1. monorepo パッケージをホストする`packages/*/*`を指すパス タイプ リポジトリを追加します。このリポジトリには、含まれている各パッケージのブランチ エイリアスも含まれます
1. プロジェクト自体にブランチエイリアスを追加する

次のGitの差分は、クリーンなAdobe Commerce インストールと上記の変更点の違いを示しています。

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

GitHubの[AntonEvers/gra-meta-foundation](https://github.com/AntonEvers/gra-meta-foundation)からサンプルコードをダウンロードして、この例で使用されているメタパッケージとサンプルモジュールを取得します。

コンポーザーのメタパッケージは、複数のコンポーザーのパッケージを1つのパッケージにまとめます。 メタパッケージが必要な場合は、メタパッケージのすべてのパッケージが、メタパッケージの「コンポーザーが必要」セクションを通じて自動的にインストールされます。

この例では、次の2つのメタパッケージがあります。

1. **antonevers/gra-meta-brand-x**:「ブランド X」を構成するすべてを含むメタパッケージ
1. **antonevers/gra-meta-foundation**：任意のブランドに常にインストールされるすべての要素を含むメタパッケージ

ブランドのメタパッケージには、基盤のメタパッケージが必要です。 ブランドメタパッケージが必要な場合は、基礎メタパッケージも自動的に必要になります。 メタパッケージの2つのcomposer.json ファイルを参照して、これらのファイルがどのように関連付けられているかを確認してください。

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

メタパッケージは、一緒に属するコードを整理する良い方法です。 メタパッケージを使用して、地域、グローバル、ブランド固有、または自社にとって意味のあるグループ化のパッケージグループを定義します。 複数のAdobe Commerceのインストールを管理する場合、matapackagesは、パッケージが必要なコンテキストを定義する安全で汎用性の高い方法を提供します。

メタパッケージは、`packages` ディレクトリ内のmonorepoに存在します。 ここでは、`vendor`のディレクトリ構造がミラーリングされています。

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

monorepoのモジュールは`packages` ディレクトリに存在します。 この方法で、Composerはパスタイプリポジトリを通じてそれらを見つけることができます。

GitHubの[AntonEvers/gra-meta-foundation](https://github.com/AntonEvers/gra-meta-foundation)からサンプルコードをダウンロードして、この例で使用されているメタパッケージとサンプルモジュールを取得します。

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

必要に応じて、`packages` ディレクトリ内に複数の名前空間を持つことができます。

開発はパッケージディレクトリで行われます。 `packages`を実行すると、`vendor` ディレクトリ内のパッケージへのシンボリックリンクが`composer update` ディレクトリに作成されます。 これにより、コードはAdobe Commerce インストールの一部になります。

追加されたモジュールを有効にするには、`bin/magento module:enable --all`または特定のモジュールのみを実行します。

これで、3つのサンプルモジュールがインストールされ、動作しているAdobe Commerceのインストールが完了しました。 モジュールがインストールされ、動作しているかどうかを確認するには、次のコマンドを実行します。

```bash
bin/magento test:gra
bin/magento test:3rdparty
bin/magento test:local
```

### パッケージ作成の自動化

自動パッケージ作成を実現するには、複数のオプションがあります。 次のようなオプションがあります。

1. [ プライベートパッケージスト ](https://packagist.com/)
1. [Monorepo Builderを簡略化](https://github.com/symplify/monorepo-builder)
1. 独自のソリューションの構築

[Private Packagist](https://packagist.com/)は、Git monorepo内のパッケージの認識を自動化し、Composerを通じて公開します。 Adobe Commerceと互換性があり、高速でメンテナンスが少なく、エラーが発生しやすいため、このガイドではPrivate Packagist オプションに焦点を当てています。

プライベート Packagistの設定方法については、このガイドの範囲を超えています。[ ドキュメント ](https://packagist.com/docs)を参照してください。

組織の同期を設定し、Git リポジトリがPrivate Packagistに自動的に同期されたら、パッケージをモノレポに変換する可能性があります。

まず、「パッケージ」タブに移動して、monorepoを見つけます。

![ パッケージ画面にmonorepo パッケージが表示されているプライベート Packagistのスクリーンショット ](/help/assets/global-reference-architecture/packagist-packages-before-multi-package.png){align="center"}

monorepo パッケージをクリックし、詳細画面の「編集」をクリックすると、次のページに移動します。

monorepo パッケージ編集ページを含む![Private Packagistのスクリーンショット ](/help/assets/global-reference-architecture/packagist-packages-edit.png)

最初の入力フィールドの下には、「マルチパッケージリポジトリを作成する」というリンクがあります。 このリンクをクリックします。

マルチパッケージ設定を使用した![Private Packagistのスクリーンショット ](/help/assets/global-reference-architecture/packagist-packages-multi-package.png)

Monorepo内でコンポーザーパッケージを見つけることができる場所を定義します。 この例では、場所は`packages/**/composer.json`です。 Private Packagistが抽出するパッケージを検索する場所を制限または拡大する場所を変更します。

「パッケージ」タブには、保存後に見つかったすべてのパッケージが表示され、monorepo自体はComposer パッケージとして表示されなくなります。

![ パッケージ画面に表示されているすべてのmonorepo パッケージを含むプライベート Packagistのスクリーンショット ](/help/assets/global-reference-architecture/packagist-packages-after-multi-package.png)

Gitのmonorepoで作成されるタグまたはブランチごとに、monorepo内のパッケージごとにComposerでバージョンが作成されます。

## 実稼動環境へのパッケージのインストール

Private Packagistの指示に従って、Private Packagistをコンポーザーリポジトリとして追加します。 Private Packagistは、packagist.orgを含むすべてのComposer リポジトリとGit リポジトリのミラーとして使用できるため、使用する必要があります。 これにより、資格情報を開発者と共有する必要がなくなり、各パッケージを完全に制御できます。 この例では、Adobe Commerce コードベースを公開するので、このベストプラクティスに従っていません。

GitHubから[GRA Monorepo Brand X](https://github.com/AntonEvers/gra-monorepo-brand-x)をダウンロードして、実稼動ストアの例をご覧ください。

実稼動ストアには`packages` ディレクトリはなく、すべてのパッケージはComposerを通じてインストールされます。 必要なパッケージは次のとおりです。

```json
    "require": {
        "antonevers/gra-meta-brand-x": "^1.0"
    },
```

しかし、Adobe CommerceとGRA全体はすべて、このメタパッケージの要件を通じてインストールされます。

## バージョン管理

monorepo内のすべてのパッケージは、monorepo自体と同じバージョンを受け取ります。 これは、アプリケーション全体の新しいバージョンを公開するものだと考えてください。 ただし、実稼動環境では、異なるmonorepo バージョンのパッケージを組み合わせてインストールできます。

## 一時的な環境

一時的な環境を使用する場合や、それを使用する予定がある場合は、monorepoが最適です。 monorepoのすべてのバージョンとブランチには、すべてのAdobe Commerce、サードパーティ、カスタムモジュールファイルが含まれています。 各ブランチに完全にインストールすれば、機能テストを含むあらゆるタイプのテストを実行できます。 別のパッケージやバルクパッケージ GRAなどの他のGRA設定では、機能テストを実行する前に、最初に動作するAdobe Commerce環境を構築する必要があります。 DevOpsの観点から見ると、monorepoは非常にシンプルにします。

## コード例

この記事のコード例は、一連のGit リポジトリで組み合わされています。このリポジトリを使用して、概念実証を試すことができます。

* monorepo リポジトリの例：<https://github.com/AntonEvers/gra-monorepo>
* 実稼動ストアの例：<https://github.com/AntonEvers/gra-monorepo-brand-x>

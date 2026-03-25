---
title: 個別パッケージ グローバルリファレンスアーキテクチャ
description: 個別のパッケージでAdobe Commerceを最適化GRA。 柔軟でバージョン管理されたパッケージ管理の設定、利点、ベストプラクティスについて説明します。
jira: KT-16727
doc-type: tutorial
duration: 594
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
topic: Architecture, Commerce, Development
badge: label="Contributed by Tony Evers, シニア・テクニカル・アーキテクト，Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="アーティスト：Tony Evers"
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: cbddc4a3-602f-4208-85cd-b906d2b81f8b
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '2101'
ht-degree: 0%

---

# 個別パッケージのグローバル参照アーキテクチャパターン

{{only-for-on-prem-commerce-cloud}}

このガイドでは、個別パッケージグローバルリファレンスアーキテクチャ（GRA）パターンを使用してAdobe Commerceを設定する方法について説明します。

個別パッケージ GRA パターンには、共通パッケージごとに1つのGit リポジトリと、各Adobe Commerce インスタンスの1つのGit リポジトリが含まれます。 一般的なパッケージは、プライベートコンポーザーリポジトリを使用してComposerを通じて公開されます。

このグローバル参照アーキテクチャパターンは完全にComposer ベースであり、すべてのComposer機能から最大限のメリットを得られるように設計されています。

![&#x200B; コードが別のパッケージ GRA パターンに格納されている場所を示す図](/help/assets/global-reference-architecture/separate-packages-gra-pattern-diagram.png){align="center"}

## このパターンの利点と欠点

利点：

* 共有コードリポジトリによるコードの再利用
* パッケージインストールの完全な柔軟性により、各GRA パッケージを個別にアップグレード、ダウングレード、またはバックポートできます
* セマンティックバージョン管理の完全サポート
* 特別なツール、複雑なインフラストラクチャ、特別な分岐戦略は必要ありません
* Composerがサポートするすべてのパッケージタイプのサポート

欠点：

* このGRA パターン内での開発は、最初は少し難しく、小さな学習曲線があります
* 同じ構成で開発されていないパッケージの組み合わせをデプロイすることが可能で、厳格なテスト手順が必要です

## Adobe Commerceを個別のパッケージ GRA パターンで設定する

### ディレクトリ構造

パッケージの分離GRA パターンを持つ完全なAdobe Commerce インストールの最終的なディレクトリ構造は次のようになります。

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
└── composer.lock
```

`app/code`、`app/i18n`、`app/design` ディレクトリは意図的に省略されています。 個別のパッケージ GRAでは、Composerからすべてのパッケージがインストールされます。 パッケージが1つのAdobe Commerce インスタンスにのみインストールされている場合でも。

### ストアリポジトリの準備

Brand Xのweb ストアを表す最初のAdobe Commerce インスタンスのリポジトリを作成します。

```bash
mkdir gra-separate-brand-x
cd gra-separate-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-separate-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

`bin/magento setup:install`でAdobe Commerceをインストールします。 結果の`app/etc/config.php`をコミットします。

### パッケージリポジトリの作成

このグローバル参照アーキテクチャパターンの各パッケージには、独自のGit リポジトリがあります。 GRA モジュール、サードパーティモジュール、ローカルモジュールを表すAdobe Commerce モジュールを含むパッケージの例を次に示します。

* <https://github.com/AntonEvers/module-example-gra>
* <https://github.com/AntonEvers/module-example-3rdparty>
* <https://github.com/AntonEvers/module-example-local>

この例を使用して、独自のパッケージを作成します。

### メタパッケージリポジトリの作成

メタパッケージは、このGRA パターンのGRA共通コードベースの範囲を制御します。 それらは基礎にあるものを定義します：どのパッケージバージョンの組み合わせが常に一緒にインストールされます。 例：

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.0",
        "antonevers/module-gra": "~1.0",
        "antonevers/module-3rdparty": "~1.0",
        "magento/composer-dependency-version-audit-plugin": "~0.1",
        "magento/composer-root-update-plugin": "~2.0",
        "magento/product-enterprise-edition": "2.4.6-p3"
    }
}
```

上記のスニペットは、メタパッケージのcomposer.jsonです。 メタパッケージにはcomposer.json ファイルのみが含まれ、他のコードは含まれないためです。 上記のコードも完全なメタパッケージです。 Git リポジトリでホストし、インストール可能なメタパッケージコンポーザーリポジトリがあります。 これには、GRA モジュールの一例とサードパーティ モジュール、およびAdobe Commerce コアが必要です。 また、次の章で説明するgra-component-foundationも必要です。

メタパッケージは、パッケージ間に依存関係を作成せずにパッケージをバンドルする方法です。 したがって、パッケージ間に技術的な依存関係がない場合でも、メタパッケージを使用すると、それらを一緒にインストールできます。 プロジェクトでこのメタパッケージが必要な場合は、メタパッケージに必要なパッケージまたはメタパッケージがインストールされます。 したがって、空白のコンポーザープロジェクトを作成し、このパッケージのみを必要とする場合、ComposerはAdobe CommerceとGRAおよびサードパーティモジュールをインストールします。

これにより、各ストアに同じ基本パッケージのセットが含まれていることを確認できます。

同様に、store xを定義するメタパッケージを定義できます。これには、GRA全体の基盤とローカルモジュールが必要な基盤メタパッケージが必要です。

```json
{
    "name": "antonevers/gra-meta-brand-x",
    "type": "metapackage",
    "require": {
        "antonevers/gra-meta-foundation": "~1.0",
        "antonevers/module-local": "~1.0"
    }
}
```

Brand-X メタパッケージはオプションです。 また、ブランドメタパッケージをスキップして、ストア Composer プロジェクトでこれらの依存関係を直接要求することもできます。 ローカルモジュールのメタパッケージを作成する利点は、ストア Git リポジトリ上に機能ブランチと機能プルリクエストがなく、パッケージリポジトリ内にのみ存在することです。 これは安全対策です。 さらに、パッケージリポジトリにセマンティックバージョン管理を適用し、メインプロジェクトで様々なGit タグを使用して、例えば名前付きリリースを追跡することもできます。 これらの機能を使用すれば、思いどおりのページを作成できます。

### ベンダーディレクトリ外のGRA基盤ファイル

ベンダーディレクトリの外部にファイルを保存する必要がある場合があります。 `.gitignore`など、`dev/` ディレクトリ内のファイルまたはドメイン検証ファイル。 magento2 コンポーネントパッケージタイプは、この目的のために設計されています。 <https://github.com/AntonEvers/gra-component-foundation>を見てください。

```json
{
    "name": "antonevers/gra-component-foundation",
    "type": "magento2-component",
    "require": {
        "magento/magento-composer-installer": "*"
    },
    "extra": {
        "map": [
            [
                "src/gitignore",
                ".gitignore"
            ]
        ]
    }
}
```

このパッケージにはmagento2-component タイプがあり、Adobe Commerceのルートディレクトリにコピーされるファイルをホストするsrc ディレクトリが含まれています。 このファイルのマッピングは、メイン Composer プロジェクトの`/src/gitignore`から`/.gitignore`にコピーします。

この方法で、ベンダーディレクトリ以外のファイルをGRA基盤の一部にすることもできます。

### GRA基盤モジュールの開発

開発はベンダーディレクトリ内で行われます。 ソースから基盤パッケージをインストールするようにComposerに依頼します。 これにより、ダウンロードしたアーカイブからパッケージをインストールする代わりに、Gitからパッケージをチェックアウトします。

```bash
rm -r vendor/antonevers/*
composer install --prefer-source
```

このコマンドを使用すると、Gitを使用してAntonevers名前空間のパッケージがチェックアウトされました。 vendor/antonevers/module-gra ディレクトリに入ると、module-gra Git リポジトリにも入ります。 ベンダーのディレクトリから直接、ブランチを作成、チェックアウト、結合し、このように開発できるようになりました。

### GRA基盤にサードパーティモジュールを含める

GRA メタパッケージにサードパーティ パッケージを追加します。 サードパーティ製コードをComposer リポジトリからインストールできない場合は、そのパッケージを作成します。 Git リポジトリを作成し、パッケージの内容（app/code/Vendor/Packageにあるすべてのもの）を追加し、リポジトリのルートに有効なcomposer.json ファイルがあることを確認します。 このパッケージをComposerからインストールできるようになりました。

## プライベート Composer リポジトリの設定

グローバルリファレンスアーキテクチャでは、プライベートリポジトリは必須ではありません。 デプロイとインストールを高速化し、composer.jsonのリポジトリ設定を削減して、セキュリティを強化できます。 他のComposer リポジトリおよびAdobe Commerce マーケットプレイスへの資格情報は、プライベートリポジトリに保存されます。 コードにバンドルされている機密性の高い資格情報や、開発者のマシンにバンドルされている資格情報は使用されなくなります。

さらに、一部のプライベートリポジトリでは、いずれかのストアの依存関係にセキュリティ上の脆弱性が含まれている場合に、メール通知などの追加機能を提供します。

遅い問題は、composer.jsonに複数のVCS リポジトリがある場合に発生する問題です。 アップグレードを行う場合、各Composer リポジトリを読み取る必要があります。50 パッケージ用の50 リポジトリを持つ場合、単一のComposer リポジトリのオーバーヘッドが50倍以上になります。

![&#x200B; コンポーザーのリポジトリーが見つからない場合に低速が発生する場所を示す図](/help/assets/global-reference-architecture/separate-packages-without-mirror-diagram.png){align="center"}

プライベート Composer リポジトリの形式でComposer ミラーを含めます。 ミラーには、他のComposer リポジトリのすべてのパッケージのコピーと、すべてのGit ホスト型パッケージが含まれています。 プライベート Composer リポジトリを使用すると、さらに細かいアクセス制御を得ることができます。

Git同期では、プライベートコンポーザーリポジトリが、Git リポジトリ内の新しいパッケージと、既存のパッケージの新しいバージョンを自動的に検出します。

Satis: <https://composer.github.io/satis/>を使用して、独自のプライベートリポジトリをホストできます。 <https://antonevers.github.io/gra-composer-repository/>のパブリックリポジトリの例を参照してください。 このリポジトリは、コードサンプルのコンポーザーリポジトリとして使用されます。 Satis リポジトリを非公開にするために、追加の対策が必要です。

設定して忘れることができる解決策があります。プライベートパッケージスト <https://packagist.com/>。これは、ComposerまたはJFrog Artifactory <https://jfrog.com/artifactory/>を記述したユーザーと同じユーザーによって作成されます。

## コードを配信

メタパッケージでは、コードを配信するための3つのステップがあります。

1. 変更をパッケージにマージし、変更したパッケージのバージョンを設定します。
2. （オプション、新しいパッケージが追加された場合のみ）新しいパッケージをメタパッケージに必要とし、メタパッケージのバージョンを設定します。
3. （オプション、新しいパッケージが追加された場合のみ） Adobe Commerceで新しいメタパッケージを必要とし、デプロイします。

デプロイメントスコープは、パッケージバージョンで制御されます。 パッケージの安定バージョンを作成すると、このパッケージを実稼動環境にデプロイする準備が整います。

新しいリリースをビルドするには、ストア全体のインストールを含むメインのComposer プロジェクトでComposer アップデートを実行します。 パッケージの最新バージョンがすべてインストールされます。

## バージョン管理

別のパッケージでのバージョン管理GRAは、Gitでモジュールをタグ付けする同義語です。 Git タグは、Composerがインストールするパッケージの番号付きバージョンを作成します。
適切なバージョン管理アプローチにより、安全性を維持しながら、パッケージは自動的に流れるようになります。

2つの例：

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "1.1.4",
        "antonevers/module-gra": "1.0.0",
        "antonevers/module-3rdparty": "1.3.89"
    }
}
```

この例は、依存関係の厳密な定義を示しています。 正確なバージョンでは3つのパッケージが必要です。 このメタパッケージをインストールしたComposerの更新プログラムは何も実行しません。 これらの3つのパッケージは、新しいバージョンが利用可能な場合でも、常にこれらの正確なバージョンにインストールされます。

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.0",
        "antonevers/module-gra": "~1.0",
        "antonevers/module-3rdparty": "~1.0"
    }
}
```

この例は、依存関係の緩い定義を示しています。 `~1.0`では、これらのパッケージのバージョンが`1.0.0`以上`2.0.0`未満で、利用可能なバージョンが最も高い場合に優先される場合、どのバージョンでもインストールできます。 バージョンの依存関係を定義する方法について詳しくは、<https://getcomposer.org/doc/articles/versions.md>を参照してください。

> ～ オペレーターは例で最もよく説明されています：`~1.2`は`>=1.2 <2.0.0`に相当し、`~1.2.3`は`>=1.2.3 <1.3.0`に相当します。

前述のパッケージの新しいバージョンをリリースするとすぐに、Composerのアップデートで自動的にインストールされます。

セマンティックバージョン管理の適用： <https://semver.org/>でセマンティック バージョン管理に関する情報をすべて学習できます。 特に、FAQは必読です。 セマンティックバージョン管理では、「1.0.0」の数値はMAJOR.MINOR.PATCHと呼ばれます。 パッケージのマイナーリリースとパッチリリースは、アプリケーションを破損することなく導入しても安全である必要があります。
パッチを自動的に含め、マイナーアップグレードを手動で選択できます。 変更を手動で行うと、オーバーヘッドが増えることに注意してください。

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.1.0",
        "antonevers/module-gra": "~1.0.0",
        "antonevers/module-3rdparty": "~1.3.0"
    }
}
```

もちろん、セマンティックバージョン管理を一貫して適用する場合にのみ、この機能はすべて常に機能します。 また、メタパッケージだけでなく、通常のパッケージの要件も依存関係を緩やかに定義する必要があります。 システム内に1つの厳密な依存関係がある場合、そのパッケージは厳密な定義に限定されます。

コンポーザーは\&lt; パッケージ名\>に依存しています。これらの厳密な依存関係を検索します。 詳しくは、<https://getcomposer.org/doc/03-cli.md#depends-why>を参照してください。

## 分岐戦略

メインブランチがパッケージのバージョンを作成する唯一のブランチである場合、様々な分岐戦略を使用して、このグローバル参照戦略パターンをサポートできます。 複数のブランチでバージョンを作成すると、バージョン間で機能がランダムに失われるリスクが生じます。 メインブランチでのみ安定したバージョンを作成します。

パッケージリポジトリ内で機能ブランチのみを作成します。 ストアのインストールリポジトリにはありません。 コンポーザーを使用するだけで、ストアに変更を加えることができます。 デプロイメントリポジトリでGit マージを行う必要はありません。

分岐の戦略やリポジトリで一般的なブランチタイプは、次のとおりです。

**機能ブランチ**: パッケージリポジトリに存在します。他の場所にはありません。

**リリースブランチ**：は、パッケージ、メタパッケージ、ストアインストールリポジトリなど、任意のリポジトリで作成されます。 リリースを計画する場合は、パッケージのリリースブランチで変更をグループ化してからバージョンを設定します。 コード名「Unicorn」でリリースを準備しているとします。 変更を加えたパッケージにrelease-unicorn ブランチを作成できます。 何かを結合してから、メタパッケージに「dev-release-unicorn as 1.4.0」を指定します。 エイリアスの詳細を確認して、ここで何が起こっているかを確認してください：<https://getcomposer.org/doc/articles/aliases.md>。

**QA/Dev ブランチ**: リリースブランチと同様です。

**メインブランチ**：すべてのリポジトリに存在する必要があり、実稼動状態または実稼動可能状態を表すブランチである必要があります。 main ブランチは、コードをリリースバージョンにタグ付けする場所です。
メンテナンスのオーバーヘッドが少ない分岐戦略を選択することを確認してください。 例えば、ホットフィックスリリース後にメインブランチをQA、UAT、リリース、または開発ブランチにマージすることは、オーバーヘッドメンテナンスタスクです。 パッケージが多ければ多いほど、より多くのリポジトリと、より反復的なオーバーヘッドタスクが発生します。

Mixu/grなどのツールを使用して、複数のGit リポジトリに対してバッチでルーチン操作を実行します：<https://github.com/mixu/gr>

## 命名規則

パッケージの個別GRA パターンでは、基盤メタパッケージで必要な場合、パッケージはGRA基盤の一部となります。 メタパッケージからパッケージを追加または削除して、パッケージを基礎に入れたり基礎から外したりします。

メタパッケージは、パッケージのインストールスコープに柔軟性を与えます。 パッケージの名前には、パッケージの意図された使用に関連する単語が含まれていないことが特に重要です。 antonevers/module-gra-store-locatorという名前は、GRA基盤からそのパッケージを取り出すと混乱することがあります。 スコープ（GRA、基礎、ローカル）の回避： 地域（EMEA、スペイン、グローバル）は避けます。 パッケージがビルドされるストアの名前は避けてください。 パッケージに追加された機能にのみ関連する名前を選択します。 そうすることで、予期せぬ将来のシナリオでも、好きなところでも再利用できます。 antonevers/module-store-locatorという名前は素晴らしいものです。

関連するパッケージがオーバービューに表示されていることを確認します。 汎用的な名前から特定の名前に作成します。 したがって、antonevers/tax-exempt-module-b2bの代わりにantonevers/module-b2b-tax-exemptを使用します。

## コード例

このブログ記事のコード例は、一連のGit リポジトリで組み合わされています。このリポジトリを使用して、概念実証を試すことができます。

* 実稼動ストアの例：<https://github.com/AntonEvers/gra-separate-brand-x>
* 基礎モジュールの例：<https://github.com/AntonEvers/module-example-gra>
* サードパーティ モジュールの例：<https://github.com/AntonEvers/module-example-3rdparty>
* ローカル モジュールの例：<https://github.com/AntonEvers/module-example-local>
* 基礎メタパッケージの例：<https://github.com/AntonEvers/gra-meta-foundation>
* ローカル メタパッケージの例（オプション）: <https://github.com/AntonEvers/gra-meta-brand-x>
* Composer リポジトリの例：<https://github.com/AntonEvers/gra-composer-repository>

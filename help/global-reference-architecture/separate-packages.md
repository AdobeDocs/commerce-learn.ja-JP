---
title: 個別のパッケージのグローバルリファレンスアーキテクチャ
description: 個別のパッケージ GRA でAdobe Commerceを最適化します。 柔軟にバージョン管理できるパッケージ管理のセットアップ、メリット、ベストプラクティスについて説明します。
kt: 16727
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
topic: Architecture, Commerce, Development
role: Architect, Developer, User, Leader
level: Beginner, Intermediate
source-git-commit: 916586d3b71b8b74baa04af2ab39abc86ec94f5b
workflow-type: tm+mt
source-wordcount: '2088'
ht-degree: 0%

---


# 個別パッケージのグローバル参照アーキテクチャパターン

このガイドでは、個別パッケージのグローバルリファレンスアーキテクチャ（GRA）パターンを使用してAdobe Commerceをセットアップする方法について説明します。

個別パッケージの GRA パターンには、共通パッケージごとに 1 つの Git リポジトリーと、Adobe Commerce インスタンスごとに 1 つの Git リポジトリーが含まれます。 共通パッケージは、Composer とプライベート Composer リポジトリを通じて公開されます。

このグローバル リファレンス アーキテクチャ パターンは完全に Composer ベースであり、すべての Composer 機能から最大限のメリットを得るように設計されています。

![ コードが個別のパッケージ GRA パターンのどこに格納されているかを示す図 ](/help/assets/global-reference-architecture/separate-packages-gra-pattern-diagram.png){align="center"}

## このパターンの長所と短所

メリット：

- 共有コードリポジトリを使用したコードの再利用
- パッケージのインストールの柔軟性が高く、各 GRA パッケージは個別にアップグレード、ダウングレード、またはバックポートできます
- セマンティックバージョニングのフルサポート
- 特別なツール、複雑なインフラストラクチャ、特別な分岐戦略は不要
- Composer がサポートするすべてのパッケージ タイプのサポート

デメリット：

- この GRA パターン内での開発は、最初は少し難しく、学習曲線は小さいです
- 同じ設定で開発されていないパッケージの組み合わせをデプロイすることは可能で、厳密なテスト手順が必要です

## 別個のパッケージ GRA パターンを使用したAdobe Commerceのセットアップ

### ディレクトリ構造

別個のパッケージ GRA パターンを持つ、完全なAdobe Commerce インストールの最終的なディレクトリ構造は、次のようになります。

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
└── composer.lock
```

`app/code`、`app/i18n`、`app/design` の各ディレクトリは、意図的に省略されています。 個別パッケージ GRA は、Composer からパッケージを 1 つずつインストールします。 パッケージが 1 つのAdobe Commerce インスタンスにのみインストールされている場合でも。

### ストアレポジトリの準備

1 つ目のAdobe Commerce インスタンス用のリポジトリーを作成します。これは、Brand X の web ストアを表します。

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

`bin/magento setup:install` と共にAdobe Commerceをインストールします。 結果の `app/etc/config.php` をコミットします。

### パッケージリポジトリの作成

このグローバルな参照アーキテクチャパターンの各パッケージには、独自の Git リポジトリがあります。 以下は、GRA モジュール、サードパーティモジュール、ローカルモジュールを表すAdobe Commerce モジュールを含むパッケージの例です。

- <https://github.com/AntonEvers/module-example-gra>
- <https://github.com/AntonEvers/module-example-3rdparty>
- <https://github.com/AntonEvers/module-example-local>

例を使用して、独自のパッケージを作成します。

### Metapackage リポジトリの作成

メタパッケージは、この GRA パターンにおける GRA 共通コードベースのスコープを制御する。 基盤の内容、つまり常に一緒にインストールされるパッケージバージョンの組み合わせを定義します。 例：

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

上記のスニペットは、メタパッケージの composer.json です。 メタパッケージには composer.json ファイルのみが含まれ、その他のコードは含まれていないからです。 上記のコードは完全なメタパッケージでもあります。 これを Git リポジトリでホストすると、インストール可能なメタパッケージコンポーザーリポジトリが得られます。 これには、1 つのサンプル GRA モジュールと 1 つのサードパーティモジュール、およびAdobe Commerce コアが必要です。 また、次の章で説明する gra-component-foundation も必要です。

メタパッケージは、パッケージ間に依存関係を作成せずにパッケージをバンドルする方法です。 したがって、パッケージ間に技術的な依存関係がない場合でも、メタパッケージを使用すると、パッケージを一緒にインストールすることができます。 プロジェクトにこのメタパッケージが必要な場合は、そのメタパッケージが必要とするパッケージまたはメタパッケージがインストールされます。 したがって、空のコンポーザプロジェクトを作成し、このパッケージのみを必要とする場合、Composer はAdobe Commerceと GRA およびサードパーティモジュールをインストールします。

これにより、各ストアに同じ基本パッケージのセットを含めることができます。

同様に、ストア x を定義するメタパッケージを定義できます。これには基盤メタパッケージが必要で、完全な GRA 基盤に加えてローカルモジュールが必要です。

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

Brand-X メタパッケージはオプションです。 また、ブランドのメタパッケージをスキップして、Store Composer プロジェクトにこれらの依存関係を直接必要とすることもできます。 ローカルモジュールのメタパッケージを作成する利点は、ストア Git リポジトリーに機能ブランチと機能プルリクエストがなく、パッケージリポジトリー内にのみある点です。 これは安全対策です。 さらに、パッケージリポジトリにセマンティックバージョン管理を適用し、主プロジェクトで異なる Git タグを使用することもできます（例えば、名前付きリリースを追跡する場合）。 それはあなた次第です。

### ベンダーディレクトリの外部にある GRA 基盤ファイル

ベンダーディレクトリの外部にファイルを保存する必要が生じる場合があります。 `.gitignore` など、`dev/` ディレクトリまたはドメイン検証ファイルに入るファイル。 magento2 コンポーネントのパッケージタイプは、この目的で設計されています。 <https://github.com/AntonEvers/gra-component-foundation> を見て。

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

このパッケージのタイプは magento2-component で、src ディレクトリが含まれています。このディレクトリには、Adobe Commerceのルートディレクトリにコピーされるファイルをホストします。 このファイルのマッピングでは、`/src/gitignore` をメインの Composer プロジェクトの `/.gitignore` にコピーします。

これにより、ベンダーディレクトリ以外のファイルを GRA 基盤の一部にすることもできます。

### GRA 基盤モジュールの開発

開発はベンダーディレクトリ内で行われます。 ソースから基盤パッケージをインストールするように Composer に依頼します。 これにより、ダウンロードしたアーカイブからパッケージをインストールする代わりに、Git からパッケージをチェックアウトします。

```bash
rm -r vendor/antonevers/*
composer install --prefer-source
```

このコマンドを使用すると、antonevers 名前空間内のパッケージが Git を使用してチェックアウトされました。 vendor/antonevers/module-gra ディレクトリに入ると、module-gra Git リポジトリも入ることになります。 ブランチを作成、チェックアウト、マージし、ベンダーディレクトリから直接、この方法で開発できるようになりました。

### GRA 基盤にサードパーティモジュールを含める

GRA メタパッケージにサードパーティパッケージを追加します。 Composer リポジトリからサードパーティ コードをインストールできない場合は、そのパッケージを作成します。 Git リポジトリを作成し、パッケージのコンテンツ（app/code/Vendor/Package 内のすべてのコンテンツ）を追加して、リポジトリのルートに有効な composer.json ファイルがあることを確認します。 Composer を通じてこのパッケージをインストールできるようになりました。

## Composer の非公開リポジトリを設定する

プライベートリポジトリは、グローバル参照アーキテクチャでは必須ではありません。 デプロイメントとインストールが高速になり、composer.json のリポジトリ設定が減少し、セキュリティが強化されます。 他の Composer リポジトリおよびAdobe Commerce Marketplace への認証情報は、プライベートリポジトリに保存されます。 コードや開発者のマシンにバンドルされている、機密性の高い認証情報はありません。

さらに、一部のプライベートリポジトリでは、追加の機能（ストアの 1 つに依存関係の 1 つにセキュリティの脆弱性が含まれる場合のメール通知など）を提供しています。

composer.json に複数の VCS リポジトリがある場合、速度の問題が発生します。 アップグレードの実行時に各 Composer リポジトリを読み取る必要があり、50 個のパッケージに対して 50 個のリポジトリを持つ場合、1 つの Composer リポジトリのオーバーヘッドの少なくとも 50 倍になります。

![ コンポーザリポジトリがない場合に速度が低下する場所を示す図 ](/help/assets/global-reference-architecture/separate-packages-without-mirror-diagram.png){align="center"}

Composer ミラーをプライベート Composer リポジトリの形式で組み込みます。 ミラーには、他の Composer リポジトリのすべてのパッケージと、Git がホストするすべてのパッケージのコピーが含まれています。 Composer の非公開リポジトリを使用すると、さらに詳細なアクセス制御が可能になります。

Git 同期を使用すると、プライベート Composer リポジトリは、Git リポジトリ内の新しいパッケージと既存のパッケージの新しいバージョンを自動的に検出します。

Satis:<https://composer.github.io/satis/> を使用して独自のプライベートリポジトリをホストできます。 公開リポジトリーの例については、<https://antonevers.github.io/gra-composer-repository/> を参照してください。 このリポジトリは、コード例のコンポーザリポジトリとして使用されています。 Satis リポジトリをプライベートにするには、さらに対策が必要です。

設定して忘れることができるソリューションがあります。Private Packagist <https://packagist.com/> は、Composer または JFrog Artifactory <https://jfrog.com/artifactory/> を書いた同じ人物によって作られています。

## コードの配信

メタパッケージには、コードを提供する 3 つのステップがあります。

1. 変更をパッケージにマージし、変更されたパッケージをバージョン管理します。
2. （オプション、新しいパッケージが追加された場合のみ） メタパッケージに新しいパッケージを必要とし、メタパッケージをバージョン化します。
3. （オプション。新しいパッケージが追加された場合のみ）Adobe Commerceで新しいメタパッケージを必要とし、デプロイします。

デプロイメントの範囲は、パッケージバージョンで制御されます。 パッケージの安定したバージョンを作成すると、このパッケージが実稼動のデプロイメントの準備が整います。

新しいリリースをビルドするには、ストア全体のインストールが含まれている Composer のメイン プロジェクトで composer update を実行します。 最新バージョンのパッケージがすべてインストールされます。

## バージョン管理

個別のパッケージでのバージョン管理 GRA は、Git のタグ付けモジュールの同義語です。 Git タグは、Composer がインストールするパッケージの番号付きバージョンを作成します。
適切なバージョン管理アプローチにより、安全を維持しながら、パッケージを自動的にフローできます。

2 つの例：

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

この例では、依存関係の厳密な定義を示しています。 正確なバージョンでは 3 つのパッケージが必要です。 インストール時にこのメタパッケージを使用して Composer をアップデートしても何も起こりません。 新しいバージョンが利用可能であっても、常にこれらの 3 つのパッケージをこれらの正確なバージョンでインストールします。

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

この例では、依存関係の緩やかな定義を示しています。 `~1.0` では、これらのパッケージが `1.0.0` 以上 `2.0.0` 未満の場合は、利用可能な最高のバージョンを優先して、どのバージョンでもインストールできます。 バージョンの依存関係を定義する方法について詳しくは、<https://getcomposer.org/doc/articles/versions.md> を参照してください。

> ～演算子は、例によって最もよく説明されています。`~1.2` は `>=1.2 <2.0.0` と同等であり、`~1.2.3` は `>=1.2.3 <1.3.0` と同等です。

前述のパッケージの新しいバージョンをリリースするとすぐに、Composer アップデートと共に自動的にインストールされます。

セマンティックバージョニングの適用。 セマンティックバージョニングについてはすべて、<https://semver.org/> で学習できます。 特に、FAQ は必読です。 セマンティックバージョニングでは、「1.0.0」の数値は MAJOR.MINOR.PATCHと呼ばれます。 パッケージのマイナーリリースとパッチリリースは、アプリケーションを中断することなく安全に導入できるはずです。
パッチを自動的に含め、マイナーアップグレードを手動で選択できます。 これにより、各軽微な変更を手動で選択することで余分なオーバーヘッドが発生することに注意してください。

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

もちろん、セマンティックバージョニングを一貫して適用する場合にのみ、常にこれが機能します。 そして、メタパッケージだけでなく、通常のパッケージの要件も依存関係を大まかに定義する必要があります。 システムに 1 つの厳密な依存関係がある場合、そのパッケージは厳密な定義に制限されます。

composer depends \&lt;package name\> と入力して、これらの厳密な依存関係を見つけます。 詳細は、<https://getcomposer.org/doc/03-cli.md#depends-why> を参照してください。

## 分岐戦略

このグローバル参照戦略パターンをサポートするために様々なブランチ戦略を使用できます（main ブランチがパッケージのバージョンを管理する唯一のブランチである場合）。 複数のブランチをまたいでバージョンを作成する場合、バージョン間で機能がランダムに失われるリスクが生じます。 main ブランチでは安定バージョンのみを作成します。

パッケージリポジトリには、機能ブランチのみを作成します。 ストアのインストールリポジトリーにはありません。 Composer を使用するだけで、ストアに変更を導入できます。 デプロイメントリポジトリで Git 結合が必要にならないようにします。

ブランチ戦略とリポジトリで共通のブランチタイプ。これらのブランチは、次の場所に存在する必要があります。

**機能ブランチ**：パッケージリポジトリには、他のどこにも存在しません。

**リリースブランチ**：任意のリポジトリ（パッケージ、メタパッケージ、ストアインストールリポジトリ）に作成されます。 リリースを計画する際には、バージョン管理を行う前に、パッケージのリリースブランチの変更をグループ化します。 コード名「Unicorn」のリリースを準備しているとします。 変更を含むパッケージに release-unicorn ブランチを作成できます。 そこに何かをマージし、メタパッケージに「dev-release-unicorn as 1.4.0」を必要とします。 エイリアスの詳細を参照して、そこで何が起こっているかを確認してください：<https://getcomposer.org/doc/articles/aliases.md>。

**QA/Dev ブランチ**：リリースブランチに似ています。

**メインブランチ**：すべてのリポジトリに存在する必要があり、常に実稼動環境または実稼動準備完了状態を表すブランチにする必要があります。 main ブランチでは、コードにタグを付けてバージョンをリリースします。
メンテナンスオーバーヘッドをほとんど発生させずに、分岐戦略を選択してください。 例えば、ホットフィックスのリリース後に main ブランチを QA、UAT、リリース、開発の各ブランチにマージし直すことは、オーバーヘッドのメンテナンスタスクになります。 パッケージが増えるほど、リポジトリが増え、オーバーヘッドの繰り返しが増えます。

mixu/gr などのツールを使用して、複数の Git リポジトリーに対するルーチン操作をバッチで実行する：<https://github.com/mixu/gr>

## 命名規則

個別パッケージ GRA パターンを使用すると、基盤メタパッケージで必要な場合、パッケージは GRA 基盤の一部になります。 パッケージをメタパッケージに追加またはメタパッケージから削除して、基盤の内外に移動します。

メタパッケージは、パッケージのインストール範囲を柔軟にします。 パッケージの名前には、パッケージの使用目的に関連する単語を含めないことが特に重要です。 antonevers/module-gra-store-locator という名前は、そのパッケージを GRA 基盤から取り出すと混乱する可能性があります。 対象範囲（GRA、基盤、ローカル）を避けます。 地域（EMEA、スペイン、グローバル）は避けてください。 パッケージの作成対象となるストアの名前は避けてください。 パッケージに追加された機能にのみ関連する名前を選択します。 そうすれば、思いがけない将来のシナリオでも、好きなところでそれらを再利用できます。 antonevers/module-store-locator という名前は優れています。

関連するパッケージが概要にまとめて表示されていることを確認します。 名前を汎用から特定にビルドします。 したがって、antonevers/tax-excempt-module-b2b ではなく、antonevers/module-b2b-tax-excempt となります。

## コードの例

このブログ投稿のコード例は、一連の Git リポジトリに組み合わされています。これを使用して、概念実証を試すことができます。

- 実稼動ストアの例：<https://github.com/AntonEvers/gra-separate-brand-x>
- 基盤モジュールの例：<https://github.com/AntonEvers/module-example-gra>
- サードパーティ製モジュールの例：<https://github.com/AntonEvers/module-example-3rdparty>
- ローカルモジュールの例：<https://github.com/AntonEvers/module-example-local>
- 基盤メタパッケージの例：<https://github.com/AntonEvers/gra-meta-foundation>
- ローカルメタパッケージの例（オプション）:<https://github.com/AntonEvers/gra-meta-brand-x>
- Composer リポジトリの例：<https://github.com/AntonEvers/gra-composer-repository>
---
title: 分割 Git グローバルリファレンスアーキテクチャを使用したAdobe Commerceのセットアップ
description: 効率的なコード管理と効率化されたデプロイメントを実現するために、分割 Git グローバルリファレンスアーキテクチャを使用してAdobe Commerceを設定する方法について説明します。​
kt: 16725
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="執筆：Adobe、シニアテクニカルアーキテクト、Tony Evers" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="寄稿：Tony Evers"
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: ac544f77-8f5f-4ad1-92b2-bdf323100c13
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '1468'
ht-degree: 0%

---

# 分割 Git グローバル参照アーキテクチャパターン

このガイドでは、分割 Git グローバルリファレンスアーキテクチャ（GRA）パターンを使用してAdobe Commerceをセットアップする方法について説明します。

この分割 Git GRA パターンには、開発用に 2 つの Git リポジトリと、Adobe Commerce インスタンスごとに 1 つの Git リポジトリが含まれます。 この例では、各インスタンスが一意のブランドを表すと想定しています。

![&#x200B; 分割 GRA パターン内のどこにコードが格納されているかを示す図 &#x200B;](/help/assets/global-reference-architecture/split-git-gra-pattern-diagram.png){align="center"}

## このパターンの長所と短所

メリット：

- 共有コードリポジトリを使用したコードの再利用
- コンポーザーの知識が限られているチームにも適したシンプルな GRA パターン
- Adobe Commerce モジュール、テーマ、および言語パックに加えて、composer-plugin、composer-metapackage、magento2-component、および patches など、このモデルを通じてあらゆるタイプの Composer パッケージをインストールできます
- リリースを段階的にリリースし、独自のメンテナンス期間に地域へのリリースを計画することが可能
- デプロイメント制御ではなく、管理目的での Git タグのサポート
- 実稼動デプロイメント内のパッケージの組み合わせが、この正確な設定で開発およびテストされることを保証します。

デメリット：

- 他の GRA パターンと比較して柔軟性が向上していない
- インスタンスごとに個別のモジュールをアップグレードまたはダウングレードすることはできません。常に GRA 全体をアップグレードまたはダウングレードしてください
- ほとんどの場合、バルクパッケージパターンは同様に単純ですが、より従来のものであるため、より適しています

## 分割 Git GRA パターンを使用したAdobe Commerceのセットアップ

### ディレクトリ構造

分割 Git GRA パターンには、開発リポジトリとインストールリポジトリの 2 種類のリポジトリがあります。 開発リポジトリには、Adobe Commerceの完全なインストールの一部のみが含まれます。 インストールリポジトリには、Adobe Commerceの完全なインストールが含まれており、デプロイメントに使用されますが、開発には使用されません。

分割 Git GRA パターンを使用した完全なAdobe Commerce インストールの最終的なディレクトリ構造は、次のようになります。

```text
.
├── .gitignore
├── app/
│   └── etc/
│       └── config.php
├── composer.json
├── composer.lock
└── packages/
    ├── 3rdparty/
    ├── gra/
    └── local/
```

`app/code`、`app/i18n`、および `app/design` ディレクトリは、Composer がこれらのディレクトリ内のコードを評価しないため、意図的に省略されます。 その結果、パッケージで宣言された依存関係は自動的にはインストールされません。 「Git GRA を分割」パターンでは、すべてのカスタムコードを `packages/` にインストールしてそのディレクトリをコンポーザリポジトリとして扱うことで、この問題を解決しています。 Composer は、`packages/` 内のパッケージを `vendor/` にシンボリックリンクします。

### Git リポジトリの準備

次の Git リポジトリの 3 つを作成します。

1. Adobe Commerce インスタンス
2. Composer によってインストールされていないサードパーティ コード
3. モジュール、テーマ、言語パックなどの形式でカスタマイズする場合：GRA

このガイドでは、これらのリポジトリに次の名前を使用します。

1. gra-split-brand-x
2. gra-split-3rdparty
3. gra-split-gra

分割 Git GRA パターン内のすべてのリポジトリは、1 つに結合されます。 Git で複数のリポジトリを結合できるようにするには、3 つのリポジトリすべてに共有履歴が必要です。 1 つのコミットで Git プロジェクトを作成し、すべてのリモートにプッシュします。

```bash
mkdir gra-split-brand-x
cd gra-split-brand-x
git init
git remote add origin git@github.com:AntonEvers/gra-split-brand-x.git
git remote add 3rdparty git@github.com:AntonEvers/gra-split-3rdparty.git
git remote add gra git@github.com:AntonEvers/gra-split-gra.git
touch .gitkeep
git add .gitkeep
git commit -m 'initialize repository'
git push -u origin main
git push 3rdparty main
git push gra main
```

一時ファイル `.gitkeep` をすべてのリモートにプッシュすると、同じコミットハッシュで同じ初期コミットが作成され、共有履歴が作成されます。 1 つのリモコンで作成した各変更は、他のリモコンに結合できます。

ここから、リポジトリーが分岐します。 gra-split-brand-x リポジトリには、ブランド固有のコードが含まれています。 gra-split-3rdparty リポジトリには、サードパーティのコードのみが含まれています。 gra-split-gra リポジトリには、すべてのカスタムコードで構成されるグローバル参照アーキテクチャ基盤のみが含まれます。

Adobe Commerceを gra-split-brand-x リポジトリにインストールします。

```bash
composer create-project --no-install --repository-url=https://repo.magento.com/ magento/project-enterprise-edition temp
mv temp/composer.json ./
rmdir temp
git add composer.json
git commit -m 'install Adobe Commerce'
git push origin main
```

gra-split-3rdparty リポジトリと gra-split-gra リポジトリに初期コミットを作成します。 最も簡単な方法は、これらのリポジトリを別々のディレクトリにチェックアウトすることです。

```bash
cd ..
git clone git@github.com:AntonEvers/gra-split-3rdparty.git
git clone git@github.com:AntonEvers/gra-split-gra.git
cd gra-split-3rdparty
mkdir -p packages/3rdparty
touch packages/3rdparty/.gitkeep
git add packages/3rdparty/.gitkeep
git commit -m 'initialize 3rd party package storage'
git push origin main
cd ../gra-split-gra
mkdir -p packages/gra
touch packages/gra/.gitkeep
git add packages/gra/.gitkeep
git commit -m 'initialize GRA package storage'
git push origin main
```

これら 2 つのリポジトリには、サードパーティのパッケージと GRA パッケージが格納されます。 Adobe Commerceの各インスタンス専用のコードが存在する場合があります。 gra-split-brand-x リポジトリにこれらのローカルパッケージを保存する場所を作成します。

```bash
cd ../gra-split-brand-x
mkdir -p packages/local
touch packages/local/.gitkeep
git add packages/local/.gitkeep
git commit -m 'initialize local package storage'
git push origin main
```

### 様々なタイプのコードの保存場所

Adobe Commerceは Composer アプリケーションです。 をインストールする場合は、常に Composer リポジトリを使用します。 モジュールベンダーが Composer リポジトリを通じてインストールを提供しない場合にのみ、サードパーティモジュールをサードパーティリポジトリに保存できます。 カスタムコードは、GRA リポジトリ内で実行することをお勧めします。 モジュールが 1 つの特定のインスタンスでのみ使用される場合、そのモジュールはローカルコードになります。

要約：

- **Adobe Commerce**: Composer リポジトリに格納されます。
- **サードパーティ モジュール**:Composer リポジトリに格納されます。
- **サードパーティモジュールのフォールバックオプション**:gra-split-3rdparty Git リポジトリに格納されます。
- **GRA 基盤コード**:gra-split-gra Git リポジトリに保存されます。
- **ローカルコード**:gra-split-brand-x Git リポジトリに格納されます。

### Composer へのパッケージ ストレージの接続

Composer では、パッケージ ディレクトリをコンポーザ リポジトリとして扱うことができます。 パッケージ ディレクトリ内のパッケージの場所を Composer に通知します。

```json
"repositories": [
  {"type": "path", "url": "packages/local/*/*"},
  {"type": "path", "url": "packages/gra/*/*"},
  {"type": "path", "url": "packages/3rdparty/*/*"},
  {"type": "composer", "url": "https://repo.magento.com"}
]
```

Composer は、3 つのストレージ ディレクトリ内で 2 レベル深い composer.json ファイルを探します。 3 つのコードストレージディレクトリ内に、`vendor/` ディレクトリと同じサブディレクトリを作成します。

例：通常パッケージが `vendor/example-corp/module-example/` にインストールされている場合は、`packages/3rdparty/example-corp/module-example/` に保存します。 Composer は、必要なときにパッケージを `vendor/example-corp/module-example/` にシンボリックリンクします。

Composer パッケージの名前空間と名前をディレクトリ構造として使用します。 例：従来から `app/code/MyCorp/MyCustomization/` に存在するモジュールの名前は、composer.json で `my-corp/module-my-customization` されています。 このパッケージを `packages/gra/my-corp/module-my-customization` に保存します。

### インスタンスリポジトリーへの新しいパッケージの組み込み

サードパーティおよび GRA リモートからのパッケージを gra-split-brand-x リポジトリに結合します。

```bash
cd gra-split-brand-x
git fetch - all
git merge gra/main 3rdparty/main
git push origin main
```

結果として、次のディレクトリ構造が得られます。

```text
.
├── composer.json
└── packages/
    ├── 3rdparty/
    ├── gra/
    └── local/
```

サードパーティおよび GRA 基盤リポジトリの変更は、ブランドリポジトリに結合されます。 このように、サードパーティと GRA のコードは 1 か所でのみ管理されます。 Git マージを使用して、変更をブランドに移動します。

Adobe Commerceでは、新しいモジュールは自動的には認識されません。 Run composer では、マージ後に新しいパッケージを追加する必要があります。 マージ後にパッケージの 1 つを更新するたびに、composer update を実行します。

### サンプルモジュールのインストール

概念実証として、サンプルモジュールをインストールして、GRA パターンがどのように機能するかを確認します。

先に進む前に、`composer install` と `bin/magento install` を実行してください。

GitHub にはのテストモジュールが 3 つあります。

1. [module-example-local](https://github.com/AntonEvers/module-example-local)
2. [module-example-gra](https://github.com/AntonEvers/module-example-gra)
3. [module-example-3rdparty](https://github.com/AntonEvers/module-example-3rdparty)

### ローカルモジュールのインストール

ローカルコードプールにモジュールを追加するのは簡単です。 モジュールをダウンロードして抽出します。 Composer で必要です。 `bin/magento` を使用して有効にし、ブランドリポジトリ内のファイルをコミットします。

```bash
cd gra-split-brand-x
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

上記の出力が表示された場合は、ブランドリポジトリに安全にコミットできます。

```bash
git add packages/local/antonevers/module-local app/etc/config.php composer.json composer.lock 
git commit -m 'add local module'
git push origin main
```

### GRA 基盤モジュールのインストールと開発

GRA リポジトリにモジュールを追加する方法は、ローカルモジュールをインストールする方法とは異なります。 デフォルトでは、コミットは origin/main （gra-split-brand-x リポジトリ）に追加されます。 GRA モジュールに対する変更は、gra-split-gra リポジトリにプッシュし、その後 gra-split-brand-x リポジトリにマージする必要があります。

### 開発環境の作成

すべてのコードプールを 1 か所に組み合わせて、開発環境を作成します。 シンボリックリンクを使用して、コードをローカル、GRA、サードパーティリポジトリに個別にプッシュできます。 まず、ブランド、GRA、サードパーティのリポジトリディレクトリの横に新しい開発ディレクトリを作成します。

```text
.
├── gra-development    # <---
├── gra-split-3rdparty
├── gra-split-brand-x
└── gra-split-gra
```

```bash
cd ..
mkdir gra-development
cd gra-development
cp ../gra-split-brand-x/composer.json ../gra-split-brand-x/composer.lock .
mkdir packages
ln -s ../../gra-split-brand-x/packages/local/ packages/
ln -s ../../gra-split-3rdparty/packages/3rdparty/ packages/
ln -s ../../gra-split-gra/packages/gra/ packages/
```

結果として、次のディレクトリ構造が得られます。

```text
.
├── packages/
│ ├── 3rdparty -> ../../gra-split-3rdparty/packages/3rdparty/
│ ├── gra -> ../../gra-split-gra/packages/gra/
│ └── local -> ../../gra-split-brand-x/packages/local/
├── composer.lock
└── composer.json
```

gra-development ディレクトリで `composer install` と `bin/magento install` を実行します。

`packages/3rdparty`、`packages/gra` および `package/local` ディレクトリから直接変更をコミットできるようになりました。 Git は、ディレクトリのシンボリックリンク先の Git リポジトリに変更をコミットします。 Git コミットコマンドは、`packages/3rdparty`、`packages/gra`、または `package/local` ディレクトリ内で発行することが重要です。 プロジェクトルートで Git コミットを実行しないでください。

### サンプルモジュールのインストール

パッケージディレクトリにサードパーティ製モジュールと GRA サンプルモジュールをインストールします。

```bash
cd packages/gra
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-gra/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-gra-main module-gra
git add module-gra
 
cd ../../3rdparty
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-3rdparty/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-3rdparty-main module-3rdparty
git add module-3rdparty
 
cd ../../..
composer require antonevers/module-gra:@dev antonevers/module-3rdparty:@dev
bin/magento module:enable AntonEvers_Gra AntonEvers_ThirdParty
bin/magento test:gra
bin/magento test:3rdparty
```

この最後のコマンドにより、モジュールがインストールされ動作していることを示す次の出力が得られます。

```bash
GRA module is installed successfully and working!
3rd party module is installed successfully and working!
```

上記の出力が表示された場合は、ブランドリポジトリに安全にコミットできます。 `git remote -v` を実行して、適切なリモートにコミットしていることを確認します。

```bash
cd packages/gra
git remote -v 
origin git@github.com:AntonEvers/gra-split-gra.git (fetch)
origin git@github.com:AntonEvers/gra-split-gra.git (push)
git add antonevers/module-gra
git commit -m 'add GRA module'
git push origin main
 
cd ../3rdparty
git remote -v 
origin git@github.com:AntonEvers/gra-split-3rdparty.git (fetch)
origin git@github.com:AntonEvers/gra-split-3rdparty.git (push)
git add antonevers/module-3rdparty
git commit -m 'add third-party module'
git push origin main
```

### インスタンスへのコードの配信

GRA とサードパーティのリポジトリを gra-split-brand-x リポジトリに結合して、コードをAdobe Commerce インスタンスに提供します。 `composer require`、`bin/magento module:enable` を実行し、結果をコミットします。

```bash
cd gra-split-brand-x
git fetch - all
git merge gra/main 3rdparty/main
composer require antonevers/module-gra:@dev antonevers/module-3rdparty:@dev
bin/magento module:enable AntonEvers_Gra AntonEvers_ThirdParty
git add app/etc/config.php composer.lock composer.json
git commit -m 'install GRA and third-party modules'
git push origin main
```

## 分岐戦略

サードパーティおよび GRA リポジトリ内のストアレポジトリのブランチ戦略を反映する場合、この GRA パターンはすべてのブランチ戦略で機能します。 リリースの場合は、3 つのリポジトリすべてで同じ名前のリリースブランチを作成します。 リリースの準備中に、リリースのブランチをストアリポジトリーで結合します。

ローカルコードとサードパーティのコードまたは GRA コードの両方を変更する必要があるチケットブランチがある場合があります。 この場合、チケットブランチは、関連するすべてのリポジトリで作成する必要があります。

サードパーティと GRA のコミットをチケット分岐内のブランドリポジトリに結合しないでください。 代わりに、開発環境の適切なブランチを、コードプールごとにチェックアウトします。 ブランドリポジトリーへの結合は、リリースを構成する際、または QA ブランチを構成する際にのみ行われます。

## コードの例

この記事のコード例は、概念実証のテストに使用できる一連の Git リポジトリとして提供されています。

- 実稼動ストアの例：<https://github.com/AntonEvers/gra-split-brand-x>
- サードパーティのコードリポジトリ：<https://github.com/AntonEvers/gra-split-3rdparty>
- GRA コードリポジトリ：<https://github.com/AntonEvers/gra-split-gra>
- ローカルモジュールの例：<https://github.com/AntonEvers/module-example-local>
- GRA モジュールの例：<https://github.com/AntonEvers/module-example-gra>
- サードパーティ製モジュールの例：<https://github.com/AntonEvers/module-example-3rdparty>

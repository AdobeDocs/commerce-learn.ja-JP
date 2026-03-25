---
title: Split Git グローバルリファレンスアーキテクチャを使用したAdobe Commerceの設定
description: Split Git Global Reference Architectureを使用してAdobe Commerceを設定し、効率的なコード管理と合理的なデプロイメントを実現する方法を説明​ます。
kt: 16725
doc-type: tutorial
duration: 515
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="Contributed by Tony Evers, シニア・テクニカル・アーキテクト，Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="アーティスト：Tony Evers"
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: ac544f77-8f5f-4ad1-92b2-bdf323100c13
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '1468'
ht-degree: 0%

---

# Split Git グローバル参照アーキテクチャパターン

{{only-for-on-prem-commerce-cloud}}

このガイドでは、Split Git Global Reference Architecture （GRA）パターンを使用してAdobe Commerceを設定する方法について説明します。

分割Git GRA パターンには、開発用の2つのGit リポジトリと、Adobe Commerce インスタンスごとに1つのGit リポジトリが含まれます。 この例では、各インスタンスが一意のブランドを表していると仮定します。

![分割されたGRA パターンでコードが格納されている場所を示す図](/help/assets/global-reference-architecture/split-git-gra-pattern-diagram.png){align="center"}

## このパターンの利点と欠点

利点：

* 共有コードリポジトリによるコードの再利用
* シンプルなGRA パターン。コンポーザーの知識が限られているチームにも適しています
* Adobe Commerce モジュール、テーマ、言語パックに加えて、このモデルを通じて、composer-plugin、composer-metapackage、magento2-component、およびパッチなど、あらゆるタイプのComposer パッケージをインストールできます
* 段階的にリリースすることが可能で、独自のメンテナンスウィンドウで地域へのリリースを計画しています
* デプロイメント管理ではなく、管理目的でのGit タグのサポート
* 実稼動デプロイメントのパッケージの組み合わせが、この正確な設定で開発およびテストされていることを保証します

欠点：

* 他のGRA パターンと比較して柔軟性が追加されない
* インスタンスごとに個々のモジュールをアップグレードまたはダウングレードすることはできません。GRA全体を常にアップグレードまたはダウングレードしてください
* ほとんどの場合、バルクパッケージパターンは、同様にシンプルですが、より従来のものであるため、より適しています

## 分割Git GRA パターンを使用したAdobe Commerceの設定

### ディレクトリ構造

Split Git GRA パターンには、開発リポジトリとインストールリポジトリの2種類のリポジトリがあります。 開発リポジトリには、Adobe Commerceの完全なインストールの一部のみが含まれます。 インストールリポジトリには、Adobe Commerceの完全なインストールが含まれており、デプロイメントに使用されますが、開発には使用されません。

Split Git GRA パターンを持つフル Adobe Commerce インストールの最終的なディレクトリ構造は次のようになります。

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

Composerはこれらのディレクトリ内のコードを評価しないため、`app/code`、`app/i18n`および`app/design` ディレクトリは意図的に省略されています。 その結果、パッケージで宣言された依存関係は自動的にはインストールされません。 分割Git GRA パターンは、すべてのカスタムコードを`packages/`にインストールし、そのディレクトリをコンポーザーリポジトリとして扱うことで、この問題を解決します。 Composerは、`packages/`内のパッケージを`vendor/`にリンクします。

### Git リポジトリの準備

次の3つのGit リポジトリを作成します。

1. Adobe Commerce インスタンス
2. Composerを使用してインストールされていないサードパーティ製コード
3. モジュール、テーマ、言語パックなどの形式でのカスタマイズ。GRA

このガイドでは、これらのリポジトリに次の名前が使用されています。

1. gra-split-brand-x
2. gra-split-3rdparty
3. gra-split-gra

分割Git GRA パターン内のすべてのリポジトリは1つに結合されます。 Gitで複数のリポジトリを結合できるようにするには、3つのリポジトリすべてに共有履歴が必要です。 1回のコミットでGit プロジェクトを作成し、すべてのリモートにプッシュします。

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

一時ファイル `.gitkeep`をすべてのリモートにプッシュすると、同じコミット ハッシュで同じ初期コミットが作成され、共有履歴が作成されます。 1つのリモートで作成された各変更は、他のリモートに結合できます。

そこから、リポジトリが分岐します。 gra-split-brand-x リポジトリには、ブランド固有のコードが含まれています。 gra-split-3rdparty リポジトリには、サードパーティのコードのみが含まれます。 gra-split-gra リポジトリには、すべてのカスタムコードで構成されるグローバルリファレンスアーキテクチャ基盤のみが含まれます。

gra-split-brand-x リポジトリにAdobe Commerceをインストールします。

```bash
composer create-project --no-install --repository-url=https://repo.magento.com/ magento/project-enterprise-edition temp
mv temp/composer.json ./
rmdir temp
git add composer.json
git commit -m 'install Adobe Commerce'
git push origin main
```

gra-split-3rdpartyおよびgra-split-gra リポジトリで初期コミットを作成します。 最も簡単な方法は、個別のディレクトリでこれらのリポジトリを確認することです。

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

これらの2つのリポジトリには、サードパーティパッケージとGRA パッケージが格納されます。 Adobe Commerceの各インスタンス専用のコードが存在する場合があります。 これらのローカルパッケージをgra-split-brand-x リポジトリに保存する場所を作成します。

```bash
cd ../gra-split-brand-x
mkdir -p packages/local
touch packages/local/.gitkeep
git add packages/local/.gitkeep
git commit -m 'initialize local package storage'
git push origin main
```

### 様々な種類のコードの保存場所

Adobe Commerceは、Composer アプリケーションです。 インストールする推奨される方法は、常にComposer リポジトリを使用することです。 モジュールベンダーがComposer リポジトリを介したインストールを提供しない場合にのみ、サードパーティモジュールをサードパーティリポジトリに保存できます。 カスタムコードの優先される場所は、GRA リポジトリです。 モジュールが1つの特定のインスタンスのみで使用される場合、そのモジュールはローカルコードになります。

まとめ：

* **Adobe Commerce**: Composer リポジトリに保存されています。
* **サードパーティ モジュール**: Composer リポジトリに保存されます。
* **サードパーティ モジュール フォールバック オプション**: gra-split-3rdparty Git リポジトリに保存されます。
* **GRA基盤コード**: gra-split-gra Git リポジトリに保存されています。
* **ローカルコード**: gra-split-brand-x Git リポジトリに保存されます。

### パッケージストレージをComposerに接続

Composerでは、パッケージディレクトリをコンポーザーリポジトリとして扱うことができます。 パッケージディレクトリ内のパッケージの場所をComposerに通知します。

```json
"repositories": [
  {"type": "path", "url": "packages/local/*/*"},
  {"type": "path", "url": "packages/gra/*/*"},
  {"type": "path", "url": "packages/3rdparty/*/*"},
  {"type": "composer", "url": "https://repo.magento.com"}
]
```

Composerは、3つのストレージディレクトリ内の2つのレベルの深いcomposer.json ファイルを探します。 3つのコードストレージディレクトリ内に、`vendor/` ディレクトリに表示されるとおりにサブディレクトリを作成します。

例：パッケージが通常`vendor/example-corp/module-example/`にインストールされている場合は、`packages/3rdparty/example-corp/module-example/`に格納します。 必要なときに、Composerはパッケージを`vendor/example-corp/module-example/`にシンボリックリンクします。

ディレクトリ構造として、コンポーザーパッケージの名前空間と名前を使用します。 例えば、従来`app/code/MyCorp/MyCustomization/`に存在していたモジュールは、composer.jsonに`my-corp/module-my-customization`という名前です。 このパッケージを`packages/gra/my-corp/module-my-customization`に保存します。

### インスタンスのリポジトリに新しいパッケージを含める

サードパーティおよびGRA リモートのパッケージをgra-split-brand-x リポジトリにマージします。

```bash
cd gra-split-brand-x
git fetch - all
git merge gra/main 3rdparty/main
git push origin main
```

その結果、次のディレクトリ構造が得られます。

```text
.
├── composer.json
└── packages/
    ├── 3rdparty/
    ├── gra/
    └── local/
```

サードパーティおよびGRA基盤リポジトリの変更は、ブランドリポジトリに統合されます。 この方法では、サードパーティとGRA コードは1か所でのみ維持されます。 Git結合で変更をブランドに移動します。

Adobe Commerceが新しいモジュールを自動認識しない。 マージ後にコンポーザーを実行するには、新しいパッケージを追加する必要があります。 マージ後にパッケージのいずれかを更新するたびに、コンポーザーの更新を実行します。

### サンプルモジュールのインストール

概念実証として、サンプルモジュールをインストールして、GRA パターンがどのように機能するかを確認します。

次に進む前に`composer install`と`bin/magento install`を実行してください。

on GitHubには3つのテストモジュールがあります。

1. [module-example-local](https://github.com/AntonEvers/module-example-local)
2. [module-example-gra](https://github.com/AntonEvers/module-example-gra)
3. [module-example-3rdparty](https://github.com/AntonEvers/module-example-3rdparty)

### ローカルモジュールのインストール

モジュールをローカルコードプールに追加するのは簡単です。 モジュールをダウンロードして抽出します。 Composerで必須にする。 `bin/magento`で有効にし、ブランドリポジトリ内のファイルをコミットします。

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

この最後のコマンドは、モジュールがインストールされ、動作していることを証明するために、次の出力になります。

```bash
Local module is installed successfully and working!
```

上記の出力が表示された場合は、ブランドリポジトリに安全にコミットできます。

```bash
git add packages/local/antonevers/module-local app/etc/config.php composer.json composer.lock 
git commit -m 'add local module'
git push origin main
```

### GRA基盤モジュールのインストールと開発

GRA リポジトリへのモジュールの追加は、ローカルモジュールのインストールとは異なります。 デフォルトでは、コミットはorigin/mainに追加されます。これはgra-split-brand-x リポジトリです。 GRA モジュールの変更は、gra-split-gra リポジトリにプッシュし、その後、gra-split-brand-x リポジトリにマージする必要があります。

### 開発環境の作成

すべてのコードプールを1か所に組み合わせた開発環境を作成します。 シンボリックリンクを使用して、ローカル、GRA、サードパーティのリポジトリに個別にコードをプッシュできます。 まず、ブランド、GRA、サードパーティのリポジトリディレクトリの横に新しい開発ディレクトリを作成します。

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

その結果、次のディレクトリ構造が得られます。

```text
.
├── packages/
│ ├── 3rdparty -> ../../gra-split-3rdparty/packages/3rdparty/
│ ├── gra -> ../../gra-split-gra/packages/gra/
│ └── local -> ../../gra-split-brand-x/packages/local/
├── composer.lock
└── composer.json
```

gra-development ディレクトリで`composer install`と`bin/magento install`を実行します。

`packages/3rdparty`、`packages/gra`、`package/local` ディレクトリから直接変更をコミットできるようになりました。 Gitは、ディレクトリがシンボリックリンクするGit リポジトリに変更をコミットします。 Git commit コマンドは、`packages/3rdparty`、`packages/gra`または`package/local` ディレクトリ内で発行することが重要です。 プロジェクトのルートでGit commitを実行しないでください。

### サンプルモジュールのインストール

サードパーティおよびGRA サンプルモジュールをパッケージディレクトリにインストールします。

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

この最後のコマンドは、モジュールがインストールされ、動作していることを証明するために、次の出力になります。

```bash
GRA module is installed successfully and working!
3rd party module is installed successfully and working!
```

上記の出力が表示された場合は、ブランドリポジトリに安全にコミットできます。 `git remote -v`を実行して、適切なリモートにコミットしていることを確認します。

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

GRAおよびサードパーティのリポジトリをgra-split-brand-x リポジトリに結合して、コードをAdobe Commerce インスタンスに配信します。 `composer require`、`bin/magento module:enable`を実行して、結果を確定します。

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

このGRA パターンは、サードパーティおよびGRA リポジトリでストア リポジトリの分岐戦略をミラーリングする場合、すべての分岐戦略で機能します。 リリースの場合は、3つのリポジトリすべてで同じ名前のリリースブランチを作成します。 リリースの準備中に、リリースブランチをストアリポジトリ上で結合します。

ローカルコードとサードパーティのコードまたはGRA コードの両方を変更する必要があるチケットブランチがある場合があります。 この場合、関連するすべてのリポジトリでチケットブランチを作成する必要があります。

サードパーティとGRAのコミットを、チケットブランチ内のブランドリポジトリに絶対に統合しないでください。 代わりに、各コードプールの開発環境の適切なブランチを確認してください。 ブランドリポジトリーへのマージは、リリースの作成時、またはQA ブランチの作成時にのみ行われます。

## コード例

この記事のコード例は、概念実証のテストに使用できるGit リポジトリのセットとして使用できます。

* 実稼動ストアの例：<https://github.com/AntonEvers/gra-split-brand-x>
* サードパーティのコードリポジトリ：<https://github.com/AntonEvers/gra-split-3rdparty>
* GRA コード リポジトリ：<https://github.com/AntonEvers/gra-split-gra>
* ローカル モジュールの例：<https://github.com/AntonEvers/module-example-local>
* GRA モジュールの例：<https://github.com/AntonEvers/module-example-gra>
* サードパーティ モジュールの例：<https://github.com/AntonEvers/module-example-3rdparty>

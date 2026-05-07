---
title: Optimizing Code Reuse with Adobe Commerce
description: Learn how to optimize code reuse in Adobe Commerce with Global Reference Architecture patterns, enhancing performance and compliance across multiple instances.
kt: 15773
doc-type: tutorial
duration: 287
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="Contributed by Tony Evers, シニア・テクニカル・アーキテクト，Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Contributed by Tony Evers"
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: 5475ade8-028c-4b24-a563-60dcda5ba93a
TQID: https://experienceleague.adobe.com/1-cE8TS4syjsMuX3VmhQu5zhFX-z3yxV-GlwxVl7eqM
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: b5f00040-57a0-4a6d-a39e-383b1936c2c9id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: b69b2659-1057-424e-8fc5-ed9e016dc554id: f8a45b24-4be7-4f1b-909b-60d06b483a20id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 1127
ht-degree: 0%

---

# Global Reference Architecture Implementation Techniques

{{only-for-on-prem-commerce-cloud}}

There are several ways to optimize code reuse with Adobe Commerce. These four implementation techniques each have their own advantages. The examples in this article are ordered from simple to more complex. Pick the strategy that best fits your project and future roadmap. A migration from one strategy to another can be time consuming.

## When to use Global Reference Architecture

Global reference architecture can be valuable, depending on the number of instances you own. An instance is a standalone installation of Adobe Commerce using its own database. Count the number of production databases to know how many instances you own. If you maintain more than one instance, or if you foresee this scenario in the future, you can benefit from global reference architecture. The more functionality the instances share, the more value a global reference architecture adds.

In any of these scenarios, it is advisable to explore using multiple instances of Adobe Commerce.

1. **Different Store Owners**: If you maintain code for multiple store owners, each with their own distinct store, separate instances may be needed to maintain their individual requirements effectively.
2. **Compliance with National Regulations**: Certain regulations mandate that customer data must be stored within specific regions. In such cases, separate instances are essential to ensure compliance with these regulations.
3. **Operational Variances Across Geographical Regions**: Operating in multiple regions may mean differing maintenance schedules and requirements. Using separate instances allows for flexibility in managing these variations efficiently.
4. **High-Intensity Flash Sales**: Stores conducting large-scale flash sales often require optimized server performance. Dedicated infrastructure provided by separate instances ensures optimal performance during such high-demand periods.
5. **ブランドと国の大きな違い**：ブランドと国の違いが大きい場合、1つのインスタンスを使用すると、一部のブランドまたは国にのみ使用されるコードが生成されます。 別々のインスタンスを構築することで、不要なコードをブランドやそれを必要としない国に提供できなくなり、パフォーマンスと安定性を向上させることができます。

## グローバル参照アーキテクチャパターン

「GRA パターンなし」の横には、4つのGRA パターンがあります。

![GRA パターンの5つのアイコン：GRA、分割、一括、分離、モノレポはありません。](/help/assets/global-reference-architecture/gra-patterns-horizontal.png){align="center"}

### GRA パターンがありません

![ 「GRAなし」を表すアイコン ](/help/assets/global-reference-architecture/no-gra.png){align="center"}

GRA パターンを使用しない場合、各Adobe Commerce インスタンスは一意のアプリケーションになります。 あるインスタンスから別のインスタンスにコードを手動で移動する以外は、コードの再利用はありません。 これらのコピーは常に発散します。 各インスタンスに同じ変更を適用しながら、期待通りに動作することを確認するのは、手間のかかる作業です。 このシナリオでは、3つのインスタンスで1つのインスタンスの3倍のメンテナンス作業が必要になります。

![3つのストアを示す図。各ストアは前者のコピーで、3つのコピーすべてで一意の開発が行われています。](/help/assets/global-reference-architecture/no-gra-pattern-diagram.png){align="center"}

### 分割Git GRA パターン

![ 「分割」 GRA パターンを示すアイコン ](/help/assets/global-reference-architecture/split-git.png){align="center"}

このパターンは、開発用のGit リポジトリと、インスタンスごとに1つのGit リポジトリで構成されます。 インスタンス内の各ファイルは、いずれかの開発リポジトリに保持されます。 彼らはGRA全体を形成する編組として一緒になります。 コードの各行は1つの開発リポジトリにのみ存在し、組み立て手法を使用してインスタンスにインストールされ、コードの再利用につながります。

![分割されたGRA パターンでコードが格納されている場所を示す図](/help/assets/global-reference-architecture/split-git-gra-pattern-diagram.png){align="center"}

### バルクパッケージのGRA パターン

![GRA パターンの「一括」を表すアイコン ](/help/assets/global-reference-architecture/bulk-packages.png){align="center"}

Adobe Commerce コアモジュールとサードパーティモジュールは、Composer リポジトリを通じて直接インストールされます。 Git リポジトリは、Composer リポジトリとして使用できます。 このパターンでは、GRA共有コードベース全体が1つまたは少数のGit リポジトリでホストされ、Composerを通じてインストールされます。 主な特徴は、複数のモジュール、言語パックまたはテーマが1つのコンポーザーパッケージでホストされ、開発が簡素化されていることです。

![ コードが一括パッケージ GRA パターンのどこに保存されているかを示す図](/help/assets/global-reference-architecture/bulk-gra-pattern-diagram.png){align="center"}

### 個別のパッケージ GRA パターン

![GRA パターンの「個別パッケージ」を表すアイコン ](/help/assets/global-reference-architecture/separate-packages.png){align="center"}

各Adobe Commerce モジュール、言語パックまたはテーマは、個別のコンポーザーパッケージとしてインストールされます。 各カスタマイズには、独自のGit リポジトリがあります。 これにより、インスタンスの構成が完全に柔軟になり、信頼性の高いComposerの依存関係管理が可能になります。 パフォーマンス最適化では、すべてのパッケージが単一のプライベートコンポーザーリポジトリにミラーリングされます。

![ コードが別のパッケージ GRA パターンに格納されている場所を示す図](/help/assets/global-reference-architecture/separate-packages-gra-pattern-diagram.png){align="center"}

### モノレポ GRA パターン

![ モノレポ GRA パターンを表すアイコン ](/help/assets/global-reference-architecture/monorepo.png){align="center"}

あらゆる開発は、単一のコードリポジトリで実施されます。 新しいバージョンのパッケージを生成し、コンポーザーリポジトリに公開します。 このパターンは、バルクパッケージアプローチの低い開発オーバーヘッドと、個別パッケージアプローチの柔軟性を組み合わせたものです。 モノレポパターンは、自動機能テストの実行にも適しています。

![ モノレポ GRA パターン内のコードの保存場所を示す図](/help/assets/global-reference-architecture/monorepo-gra-pattern-diagram.png){align="center"}

## GRA パターンの選択

GRA パターンの選定は、プロジェクトの複雑さ、柔軟性の必要性、開発チームの適応能力を評価することによって行われます。

Adobe Commerceをほとんど利用していないチームは、シンプルに取り組むことができます。 ただし、プロジェクトの特性により、より複雑なGRA パターンが必要な場合は、妥協しないでください。

各パターンに関連する一般的なプロジェクト特性：

1. **GRA パターンがありません**：拡張する計画のないAdobe Commerceの1つのインスタンス。 Adobe Commerceの複数のインスタンス間の共通性を最小限に抑えることができます。

2. **Git GRA パターンの分割**: カスタマイズのためにComposerを避けたいチーム。ほとんどの場合、バルクパッケージパターンはGitの分割に適したパターンです。

3. **バルクパッケージ GRA パターン**：相互依存関係の高いカスタマイズ コードベース。 インスタンスには、すべてカスタムパッケージの非常に類似した組み合わせがあります。 個別のパッケージのプロモーションやデモートは頻繁におこなわれません。 コード管理の経験が少なく、簡素化が必要なチーム。

4. **個別パッケージ GRA パターン**：柔軟なリリーススコープ管理が必要です。 今後5年間で50個以下のカスタムパッケージを提供する予定です。 共通コードのグローバルおよびリージョンのレイヤーが存在する可能性があります。 Monorepo パターンに移行する予定はありません。 チームは技術的に熟達しており、厳格なプロセス順守を遵守しています。

5. **モノレポ GRA パターン**：個別パッケージ GRA パターンのすべての特性。 今後5年間で50以上のパッケージが見込まれています。 大規模な自動テストの必要性： 一時的な環境のサポート。 エンタープライズ規模の複雑なコードベースを維持するため、メンテナンスコストを迅速に拡張する必要がある。

### 選択を誤った場合のコスト

あるパターンから別のパターンへの移行は可能です。 下の図は、あるパターンから別のパターンへの移動の影響レベルを示しています。 緑の線は影響が小さいことを示し、黄色とオレンジの線は中程度に複雑な移行を示します。

![4つのGRA パターンすべての間の矢印を示す図。一方から他方へ移動する難易度を示します。](/help/assets/global-reference-architecture/wrong-choice.png){align="center"}

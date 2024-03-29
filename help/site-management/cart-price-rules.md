---
title: 買い物かごの価格ルールの作成
description: 一連の条件に基づいて買い物かごに割引を適用する買い物かごの価格ルールを作成する方法を説明します。
doc-type: feature video
audience: all
activity: use
last-substantial-update: 2022-12-28T00:00:00Z
feature: Configuration, System, Customers, Shopping Cart
topic: Commerce, Administration
role: Admin, Leader, User
level: Beginner, Intermediate
exl-id: ae8cab73-8a8b-4266-8205-b7397633e9bf
source-git-commit: 273119420a7051b1833d9b796bdce36e17d893c7
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 0%

---

# 買い物かごの価格ルールの作成

買い物かごの価格ルールは、一連の条件に基づいて、買い物かご内の品目に割引を適用します。 割引は、条件が満たされた場合や、顧客が有効なクーポンコードを入力した場合に、自動的に適用されます。 適用すると、割引は買い物かごの小計の下に表示されます。 買い物かごの価格ルールは、シーズンやプロモーションの必要に応じて、ステータスと日付範囲を変更することで使用できます。

## このビデオは誰のためのものですか？

- e コマースマーケター
- Web サイトマネージャー

## ビデオコンテンツ

>[!VIDEO](https://video.tv.adobe.com/v/343835?quality=12&learn=on)

## 価格の表示の問題

提供された割引を各行項目に表示する必要がある一意のシナリオがいくつかありますが、値が正確に一致しない場合があります。 買い物かごの価格ルールの割引が複数の製品に適用され、値が小数点以下 2 桁に均等に分割されない場合が原因です。

>[!BEGINSHADEBOX]

買い物かご価格ルール=買い物かご内の 2 つの製品に適用された 10%割引価格ルールが有効になる条件：買い物かご内の合計品目は 2 アクション製品価格割引のパーセントを適用し、その割引額は 10 です

2 個の項目が買い物かごに追加され、それぞれ 19.95 ドル

割引額を取得するには、製品価格に 0.1 を掛けます。

19.95 x 0.1 = 1.995

これは問題です。2 つではなく、小数点以下 3 桁です。 これをドルに換算するのは問題になりました

>[!ENDSHADEBOX]

### 解決策

この問題の影響を受ける唯一の人物である Web サイトの所有者について考えると、各商品をドルで注文した場合の表示が最も適切であると判断されました。 注文額全体が正しく計算されるように、最初の項目を切り上げ、その他の項目では 3 番目の小数点以下を切り上げることにしました。 次のシナリオを確認します。

>[!BEGINSHADEBOX]

上の買い物かごルールと同じ 10%割引を適用 19.95 の買い物かごに 2 つの製品を追加

各製品は、割引で$1.995 を受け取る必要があります製品 1 - 19.95 x 0.1 = 1.995 2 - 19.95 x 0.1 = 1.995

顧客に対する割引として総計 3.99 が提供されます

管理者でストアの所有者に行項目を表示する場合は、最初の項目を調整し、2.000 に切り上げる必要があります。2 番目の項目は、3 番目の小数点の製品 1 = 2.00 製品 2 = 1.99 です。

合計した時点での 2 つの製品の合計割引額は、顧客に提供される実際の割引額と一致します。
>[!ENDSHADEBOX]

次に、このシナリオを持つ注文のスクリーンショットを示します。

![異なる値を持つ順序付き項目を表示する管理ビュー](../assets/commerce-admin-cart-price-rule-values-different.png)

### その他の潜在的な解決策と、それらが使用されなかった理由

>[!BEGINSHADEBOX]

上の買い物かごルールと同じ 10%割引を適用 19.95 の買い物かごに 2 つの製品を追加

各製品は 1.995 ドルの割引を受ける必要がありますが、単に切り上げれば、割引が多すぎます。

製品 1 - 19.95 x 0.1 = 1.995 製品 2 - 19.95 x 0.1 = 1.995

すべての項目を切り上げるには変換製品 1 新しい値は 2.00 製品 2 新しい値は 2.00 です

実際には、顧客に対する割引として総計 3.99 が提供されましたが、合計すると 4.00 ドルが与えられたことが示され、それは間違いです。

2.00 + 2.00 = $4.00

>[!ENDSHADEBOX]

同様に、すべての項目に対して 3 番目の小数点が下落した場合、提供される割引が少なすぎて表示されます。

>[!BEGINSHADEBOX]

上の買い物かごルールと同じ 10%割引を適用 19.95 の買い物かごに 2 つの製品を追加

各製品は、割引で$1.995 を受け取る必要がありますが、3 番目の小数点以下を落とすと、次のようになります。 Product 1 - 19.95 x 0.1 = 1.995 Product 2 - 19.95 x 0.1 = 1.995

すべての項目の 3 番目の小数点をドロップするには変換します。 1 商品 1 新しい値は 1.99 商品 2 新しい値は 1.99 です。

実際には、顧客に対する割引として総計 3.99 が提供されましたが、3 桁目を落とすと、$3.98 が与えられたことが示され、それは間違っています。

1.99 + 1.99 = $3.98

>[!ENDSHADEBOX]


## その他のリソース

- [買い物かごの価格ルールの作成 — [!DNL Commerce] マーチャンダイジングとプロモーションガイド](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-create.html)
- [クーポンコード — [!DNL Commerce] マーチャンダイジングとプロモーションガイド](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-coupon.html)

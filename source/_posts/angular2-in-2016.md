title: 2016年のAngular2について
date: 2016-01-07 12:07:19
tags:
- Beta.1
- Planning
- Meeting
---

2015年12月にBeta.0バージョンをリリースしたあと、年末年始の休暇もあってしばらくおとなしかったAngular2開発チームですが、今週から本格的にBeta.1に向かって活動を再開しました。
新年最初のミーティングが1/4に行われたので、その議事録から今後の短期〜中期のAngular2の展望を予想します。

[議事録](https://docs.google.com/document/d/150lerb1LmNLuau_a_EznPV1I1UHMTbEl61t4hZ7ZpS0/edit#)

### Post beta.0 (and post ngmaterial 1.0) check-in and what's next? (Brad/Igor)
まずはじめに、angular2 beta.0とangular-matetial 1.0がリリースされたので、次に何を行うべきかを話し合っています。
挙げられたのは次の項目

- パフォーマンスの向上とライブラリのファイルサイズ圧縮
- FormやRouterについてのフィードバックを公開
- angular2-materialの開発
- GoogleでのTypeScriptサポートを拡充する
- Angular2のトレーニング用のコンテンツの拡充
- 開発ガイドとAPIリファレンス、exampleのテスト

この他に重要なものは個別に下で述べられています。

### ng-merry-cleanup 2015 (Igor)
ng-merry-cleanup（merry-christmasとかけた？）という名前で、2015年柱に溜まったIssueやPRややり残し、負債、いろいろなものを処理するミニプロジェクトが立ち上がっています。
[ドキュメント](https://docs.google.com/document/d/13RcOCClz_FyKSthZgFTDnHUYfRoHnyCcz5MQFarLSAk/edit)

このプロジェクトの目的は開発チームの生産性向上で、直接我々ユーザー側に影響することはほとんどないですが、唯一npm3対応だけは影響がありそうです。
npm3対応はBrian氏が地道にマイグレーション作業を行っています。

### Interactive Mary Poppins (Matias)
GitHub上のbotの話題です。Mary PoppinsというのはAngularJSのプロジェクトで動いているGitHubのbotで、PRの受付などいろいろ自動化してくれるやつですが、それをさらに強化したものを検討中ということです。

### Caretaker (Igor)
旅行中などに誰が管理人を代行するのかなど、プロジェクトの運営に関する話題です

### The Journey to 10K (Igor)
Angular2のファイルサイズを2016年中に10kB以下まで減らそうという計画が立ち上がっています。海外のモバイルは未だ2G回線などの需要もあるので、それらを意識した取り組みです。

現在のライブラリの仕組みでは、アプリケーションの規模にかかわらずAngular2のすべてのコードをロードしてしまうので、とても小さなアプリケーションでもペイロードのサイズは肥大化してしまいます。これを解決し、使用する機能に応じて小分けにインポートできる仕組みを目指しています。

現在のAngular2は164kB (angular2.min.js + dependencies)ですが、3月までのQ1の間にAngularJSと同じ50kBまで減らせるようにする見込みです。

### Angular Dream CI (Igor)
現在のTravis CIが不便らしく、いろいろやるようです。詳細は後日出てくるらしい。

### Analytics (Igor)
Google Analyticsを使って開発チームの生産性を向上させる計画があるようです。

### pre-TC39 briefing (Igor)
Angular2チームとMSが合同で、Zones(zone.js), Decorators(TypeScript decorators), Module Loader(おそらくSystemJSベース)をTS39のプロポーザルとして持ち込もうとしているようです。

### Even faster Angular 2 (Tobias)
最後にパフォーマンス向上についての話題です。
近いうちにオフラインコンパイルという新しい仕組みが導入され、ライブラリからAngular2のコンパイラを分離することでスタートアップ時間やペイロードサイズの圧縮が実現できるようです。

プロポーザルのドキュメントは[こちら](https://docs.google.com/document/d/11r8IuS4xDyhVSEBp7fDYo7aiLYsLEXKs4lPd36umUGM/edit)

DIに関しても同様に静的に解決してしまうことでペイロードサイズをどんどん減らそうという計画もあります。

コンパイル結果はヒューマンリーダブルで読みやすく、出力後のコードをさらにオンライン処理（今までどおりのAngular2の使い方）に持ち込むこともできる予定です。
デバッグも行いやすくなり、ComponentのHTMLテンプレート中にブレークポイントを作れるようになるかもとのこと。

---

4日のミーティングの議事録はここまでですが、11日に2016年のAngular2のプランについてアナウンスをする予定らしいので、そこでまた詳しい話があるかもしれません。期待しましょう。

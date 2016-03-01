title: 週次ミーティング、海外記事の紹介他
date: 2016-03-01 21:55:29
author: laco0416
tags:
- Meeting
- AngularJS
- Event
- Planning
- Blog
- ng-japan

---

こんにちは、らこです。今回は内容盛りだくさんです。

## 週次ミーティング

まずは週次ミーティングについて。ここ2週間ミーティングもリリースもなかったのはオフサイトを行っていたかららしく、今週から完全復活の模様です。

### Batarangle
AngularJSのデバッガとして有名なChrome拡張のBatarangが、Angular 2対応した「Batarangle」をリリースしています。

[Batarangle.io](http://go.rangle.io/batarangle)

近いうちにBatarangleを使ったデバッグに関するドキュメントを公式に出す予定だそうです。

### オフサイトについて
今後の長期的な計画についてオフサイトで話し合ったようです。

- アップグレードを簡単にすることでAngular 2を古くならないようにする(古いバージョンのAngularが使われ続けないようにする)
- ビジュアルデザイナー向けのツールを提供する

という2つの大きな展望が掲げられたようです。

### ビルド環境とCIについて
先週AngularチームはCI環境の改善について多くの時間を割いたらしく、頻繁に落ちるテストを直したり無効にしたりと忙しかったようです。
(これで先週はPRがほとんど処理できてなかったゴメンよということらしい)
現在PRを出してもなかなかCI結果が緑にならず、安心してマージできないことが多々あります。
特にSauce Labのテストは特に頼りないので、なんとかしたいねという話をしています。

### マイルストーンについて
今週末までに、Angular 2最終版(Stableリリースのことだと思われる)までのマイルストーンを発表するらしいです。超楽しみ！

### Routerのアップデートについて
Routerのアップデートがやはり来るようです。主に作業しているのはBrianとNaomiとIgorです。(Brianは今バケーションとのこと)
ただしアップデートはできるだけ段階的に行い、劇的な破壊的変更は出さないようにするとのこと。新しいRouterの機能として計画されているのは、

- 静的なリンクチェック(`routerLink`が静的解析可能になる？)
- `canActivate`(今はデコレータで提供されているが、ライフサイクルとして再実装されるっぽい=>DIが使えるようになる)
- 非同期モジュール読み込みのサポート(これは一番最後に実装する予定)
- もっとわかりやすいRouter API(Router Link DSLの廃止？)

今月からはRouterが大きく変わりそうです。要注目。

補足情報として、`ui-router`がAngular 2に対応し始めたようです。

[feat(ui-router-ng2): Initial angular2 support · angular-ui/ui-router@217de70](https://github.com/angular-ui/ui-router/commit/217de70449c7870057d440d161752f1945dc4937)

まだ正式リリースはされていませんがnpmでインストールすればng2用のソースを使えるようです。興味のある人は試してみては。
個人的にはComponent Routerでお腹いっぱいです。

### 溜まってるPRについて
先述の通り先週はいろいろと忙しくてPRを見られてなかったので、今週はその消化に充てたいとのこと。

### AngularJSの使われ方に関する調査
グーグルのクローラを使って分析したところ、現在150000ドメインのウェブサイトがAngular 1を使っており、
ページ数でいえば200000000件にも及ぶとのこと。

ただし使われているバージョンはまだ多くが1.4への移行中であり、1.5がリリースされたにもかかわらずまだ0.9.16を使っているサイトもあるらしいです。
この問題を解決するために現在の安定バージョンへのアップグレードを推奨するポリシーをつくります。
明日、明後日にも作成されるらしいです。

### ngAnimateについて
Angular 2用の**ngAnimate**がいよいよあと数週間でプロトタイプを完成させるようです。主な機能は以下のとおり

- アノテーションレベルでのアニメーション記述
- CSSパーサの実装
- シーケンス処理
- Web Animations APIとの連携

ngAnimateを使うためのコードは驚くほど少なく、スタイルとアニメーションが分離されるのでシンプルな記述ができます。
DOMの計算も少なく、とてもハイパフォーマンスなアニメーションを実装しています。
Web Animations APIについては、現在ChromeとFirefoxでだけサポートされているので他のブラウザではpolyfillを必要とします。
ただしpolyfillをそのまま同梱すれば楽ではありますがサイズが12kbと大きいので、最小限のものを実装し直すつもりだそうです。

## AngularJSについて
Angular 1.5.1が近日中にリリースされる予定です。
また、Angular 1.6系の計画も進んでいます。機能としてはAngular 2につなげるためのものですが、破壊的変更がある場合はまず1.6で実装し、
その後1.5系にバックポートしていくという方針らしいです。1.5系をLTS的に扱いたい雰囲気を感じます。

## ng-japan
今月の21日に開催される **ng-japan** の参加申し込みが今日行われ、あっという間に260人のチケットが埋まってしまいました。

[ng-japan - The Angular conference in Tokyo, Japan (2016/3/21)](http://ngjapan.org/)

募集開始前にng2-infoでも告知しようと思っていたのですがすっかり忘れていました。すみません :P
一応キャンセル待ちはできるので、万が一に備えて応募しておくと良いと思います。

またこれはまだ未定なのですが、ng-japanの翌週にアフターイベントということで軽いミートアップを開催しようと思っています。
「ng-japanで学んだことを試してみたけどわからなかった」とか、「ng-japan行けなかったけど聞きたいことがあった」とか、
そういう部分をフォローするためのアフターイベントにするつもりです。また詳しいことが決まり次第告知します。

## 海外ブログ紹介
続々と海外のブログで面白い記事が出てきています。

### [Components with Custom Templates in Angular 2](http://www.michaelbromley.co.uk/blog/513/components-with-custom-templates-in-angular-2)
ComponentのテンプレートHTMLを動的に差し替える方法についての実践的な内容です。

### [How to build Angular 2 apps using Observable Data Services - Pitfalls to avoid](http://blog.jhades.org/how-to-build-angular2-apps-using-rxjs-observable-data-services-pitfalls-to-avoid/)
Observableを活用したデータサービスを実装する際の落とし穴と避け方についての内容です。
合わせて読みたいのはこちら

- [HTTP Requests Are Cold / Lazy Streams In Angular 2 Beta 6](http://www.bennadel.com/blog/3035-http-requests-are-cold-lazy-streams-in-angular-2-beta-6.htm)
- [RxのHotとColdについて - Qiita](http://qiita.com/toRisouP/items/f6088963037bfda658d3)

RxJSのObservableの*Hot*と*Cold*の違いを理解することが大事です。
特に最も重要なAjaxは、`http.get`の戻り値のObservableがColdであることに留意しないと挙動に悩まされるでしょう

----
今週はおそらくBeta.8がリリースされるでしょう。それではまた。


参考URL

- [Angular Weekly Meeting - Google ドキュメント](https://docs.google.com/document/d/150lerb1LmNLuau_a_EznPV1I1UHMTbEl61t4hZ7ZpS0/edit#heading=h.v9yw0ynxk8hq)
- [Angular 1.x Meeting Notes - Google ドキュメント](https://docs.google.com/document/d/1xKEbydyUEOQ_gTbcbxy_k2myctG8EiVbeMgLgXTxIc0/edit#heading=h.yu4zcypn0qmi)
- [Angular2 - Animation Meeting Notes - Google ドキュメント](https://docs.google.com/document/d/1j6Crqk5rLwJH_iahUStnlaXNfuJZH2cwOZwAYq0p218/edit#heading=h.qr27868rrp3z)

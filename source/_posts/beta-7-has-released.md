title: Angular2 Beta.7リリースとng-nl情報
date: 2016-02-20 00:13:27
author: laco0416
tags:
- Release
- Beta
- Event

---

こんにちは、らこです。
今週は週次ミーティングもなく、木曜日にリリースがなかったので次は来週かなと表いましたが、1日遅れでBeta.7にアップデートされました。
とはいえ今回のリリースはバグ修正のみで、そのバグも小さなものなので、変更点の解説は行いません。
CHANGELOGのリンクだけを貼っておくので、気になる方はチェックしてみてください。

[2.0.0-beta.7 (2016-02-18)](https://github.com/angular/angular/blob/master/CHANGELOG.md#200-beta7-2016-02-18)

ただ、CHANGELOGには載っていないのですがBeta.7でpeerDependenciesが更新されています。
RxJSは5.0.0-Beta.2に、Zone.jsは0.5.15に上がっています。
特にRxJSの方は破壊的変更があるので、忘れないようにアップデートしましょう。
`fromPromise`から`PromiseObservable`への改名がされているだけなので、忘れていてもビルドが通らなくて気付くはずです。

[build(package): bump rxjs to 5.0.0-beta.2 · angular/angular@46d9c87](https://github.com/angular/angular/commit/46d9c87ddc7071e8bc6c21032171610e6b5f6e5a)

----

リリースノートの話が特になかったので、今回は **ng-nl** について紹介します。
昨日、オランダでng-nlというAngularのコミュニティイベントが開催されました。

[NG-NL 2016](http://www.ng-nl.org/)

公式サイト上ではまだセッションやスライドは公開されていないのですが、何人かの登壇者がTwitter上で共有しているので見ることができます。

### [Angular 2 Change Detection Explained](http://pascalprecht.github.io/slides/angular-2-change-detection-explained/#/)
Pascal Precht氏によるChange Detectionの解説です。
Angular2を学習する上で必読といえるレベルのスライドです。ぜひ読んでみてください。

### [Angular 2 and the Single Immutable State Tree // Speaker Deck](https://speakerdeck.com/cironunes/angular-2-and-the-single-immutable-state-tree)
Ciro Nunes氏によるImmutableの利点についてのセッションです。
Angular2におけるMutableなモデルの欠点と、Immutableなモデルを使うことによる利点が説明されており、
PascalPrechtのCD解説の後に読むとよりわかりやすいと思います。

### [Introduction to RxJS 5 by Gerard Sans](http://slides.com/gerardsans/ng-nl-rxjs5#/)
Gerard Sans氏によるRxJSの解説です。
非同期処理の歴史やRxJS/Observableの思想から、Angular2がRxJSを使っていることによる利点を細かく解説しています。

このスライドと一緒に、去年のAngular ConnectのRxJSセッションも読んでおくと良いでしょう。

[RxJS In-Depth - AngularConnect 2015](http://www.slideshare.net/benlesh1/rxjs-indepth-angularconnect-2015)

### [Angular 2 and Relay // Speaker Deck](https://speakerdeck.com/sfroestl/angular-2-and-realy)
Sebastian Fröstl氏によるAngular2とRelay/GraphQLの組み合わせに関するセッションです。
RelayとGraphQLによるクライアント・サーバー間のコミュニケーションの記述と、
それをAngular2と併用可能であることが紹介されています。

### [How to write an Angular 2 library by Olivier Combe](http://slides.com/ocombe/ngnl2016#/)
Olivier Combe氏によるAngular2のライブラリを書く方法のセッションです。
残念ながらスライドはあまり詳しくは書かれておらず、セッションの録画が欲しくなります。

----

他にも多くのセッションがあったはずなので、見つかり次第紹介していきます。

ところで、来週の2/24に東京で開催されるAngular2勉強会で、Angular2とライブラリの話で登壇する予定です。

[第1回Angular2勉強会 #ng2Curry - connpass](http://lig.connpass.com/event/26115/)

すでにconnpassの参加枠は埋まっているので今から参加はできませんが、参加予定の方はお楽しみに。

それではまた。
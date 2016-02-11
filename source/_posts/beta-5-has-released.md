title: Angular2 Beta.5リリースと今週のニュース
date: 2016-02-11 10:23:58
author: laco0416
tags:
- Release
- Beta.5
- AngularJS
- ComponentRouter
- Meeting
- BreakingChange
---

こんにちは、らこです。毎週リリースになってからng2-infoも週刊になりつつあります。
本来なら今週はBeta.4のリリースだったのですが、
Beta.4にバージョンが上がった直後に変更漏れが見つかったので、即時修正されてBeta.5としてリリースされています。
機能的にはほとんどBeta.3と変わらないのですが、多くの修正と開発環境の改善が盛り込まれているので
早めにアップデートしたいバージョンです。

それでは注目すべき変更点を挙げていきます

[CHANGELOG](https://github.com/angular/angular/blob/c7261c295c130b3ec35687bb07b27c553c8f4961/CHANGELOG.md#200-beta5-2016-02-10)

----

### angular1_router: allow component to bind to router
[0f22dce](https://github.com/angular/angular/commit/0f22dce)

先週から公開されているAngularJS用のComponentRouterがAngular 1.5で導入されたcomponentに対応しました。

[excellalabs/ngComponentRouter: Angular 2 Component Router for Angular 1](https://github.com/excellalabs/ngComponentRouter)

もはやngRouteを使う理由はないですが、ui-routerからどのタイミングでどう移行すべきかこれから検討が必要です。

### typings: install es6-shim typings to a location users can reference.
[f1f5b45](https://github.com/angular/angular/commit/f1f5b45)

今週の破壊的変更です。angular2パッケージが外部に公開する型定義ファイルが大きく変わりました。
今回の変更ではes6-shimの型定義がangular2の型定義から参照されなくなり、
TypeScriptの推奨コンパイルターゲットがes6になりました。

es6向けにコンパイルする場合はTypeScriptのlib.d.tsからPromiseなどのES6型定義が提供されます。
今までどおりes5向けにコンパイルする場合はユーザーが明示的にes6-shimの型定義を読み込む必要があります。
その際の型定義ファイルも`browser.d.ts`という型定義ファイルがangular2パッケージに同梱されているので、自分でインストールする必要はありません。

```
///<reference path="node_modules/angular2/typings/browser.d.ts"/>
```

また、テスト用の型定義も本体には含まれなくなっているので、testingモジュールを使う際は
[typings](http://github.com/typings/typings)を使って、jasmineやangular-protractor、selenium-webdriverの型定義を個別にインストールしないといけません。

### zone.jsの依存バージョンが0.5.14に上がった
[fix(release): need to depend on latest rxjs and zone.js · angular/angular@fc88777](https://github.com/angular/angular/commit/fc887774da144db3dd2c3ff0adf418b7ca97730f)

Beta.3で問題となったzone.jsのpostinstall問題が0.5.14で解決しました。
Beta.5からはzone.jsがtsdを要求することはありません。

[chore(build): tsd install on prepublish · angular/zone.js@877af62](https://github.com/angular/zone.js/commit/877af62a6e39e4dd024517d05541b2f9e81d1bbd)

### async: handle synchronous initial value in async pipe
[26e60d6](https://github.com/angular/angular/commit/26e60d6)

AsyncPipeが初期値を与えられている場合に即反映するように修正されました

### compiler: use event names for matching directives
[231773e](https://github.com/angular/angular/commit/231773e)

Directive名と同じ名前のOutputを定義したときに、イベントとして直接Directiveのセレクタが使えるようになりました。

以前までは次のようにDirectiveとイベントハンドラを別に書く必要がありました

```
@Directive({selector: '[customEvent]'})
class EventDir {
  @Output() customEvent = new EventEmitter();
}
```

```
<div customEvent (customEvent)="doSomething()">click me</div>
```

今後はselectorとOutputが一致する場合はイベントハンドラだけで宣言できるようになります

```
<div (customEvent)="doSomething()">click me</div>
```

### upgrade: fix infinite $rootScope.$digest()
[7e0f02f](https://github.com/angular/angular/commit/7e0f02f)

UpgradeAdapterを使った時の$digestループ処理にバグがあり、無限にループし続けることがあった問題が修正されました。

----

今週の週次ミーティングの内容もざっくりと紹介します。

### Payload reduction "progress"
Angular2のペイロードサイズの削減が本格的に始まる模様です。
Hello Worldアプリが10kBになるまでとことん減らしていくとのこと。

### Popularity report (Brad)
Angular2の普及度合いについてのレポートです。
angular.ioは1ヶ月で292000ビューを記録したらしいです。
ng-confとAngular Connectで大々的にアピールしてユーザー数を増やしたいという計画。

### PHP rendering update
少し驚きですが、Angular UniversalがHTMLだけでなくPHPもレンダリングできるように拡張する予定があるそうです。
詳細は未定ですが既存のPHPアプリケーションのバックエンドを差し替えられるようなものを計画中とのこと。

### Template services plan
これも驚きの計画で、TypeScriptの入力補完候補などを提供しているLanguage Servicesにプラグイン機構を導入し、
Angular2のテンプレートHTML中で入力補完が可能になるプラグインを提供しようというものです。
スクリーンキャストではありますが、実際にAtom上で動作している様子が見られます。

[TypeScript extensibility · Issue #6508 · Microsoft/TypeScript](https://github.com/Microsoft/TypeScript/issues/6508)

[TypeScript/plugin-ngml.ts at ngml · billti/TypeScript](https://github.com/billti/TypeScript/blob/ngml/src/services/plugin-ngml.ts#L1785)

TypeScript開発チームとともに作っているだけあり、やることが大胆です。
Language Servicesで対応されれば様々なエディタで入力補完ができるようになるので楽しみにしましょう。

### Angular 2's HTTP moving to it's own repo
Angular2のhttpモジュールは今まではangular/angular内で開発されていましたが、Issueを分けたい、将来的に開発場所も分けたいということで
リポジトリが独立しました。

[angular/http: Angular 2.0 HTTP Module](https://github.com/angular/http)

今はまだangular/angularで開発されたものがコピーされているだけです。

### Electron
Angular2でのデスクトップアプリ開発を支援するために、angular/electronというプロジェクトが動くようです。

----

結構情報盛りだくさんの一週間でした。Beta.5は結構アタリのバージョンだと思うので特に心配なくアップデートしてよさそうです。

それではまた。

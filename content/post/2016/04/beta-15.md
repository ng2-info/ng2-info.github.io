+++
title= "Beta.15のリリースと記事の紹介"
date= "2016-04-15T12:12:13+09:00"
tags =["Beta","Release"]

+++

どうも、らこです。今週はAngular 2.0.0のBeta.15がリリースされました。まずは主な変更点を紹介します。

<!--more-->

---

## [Beta.15](https://github.com/angular/angular/blob/master/CHANGELOG.md#200-beta15-2016-04-13)

### テンプレートキャッシュの実装
[feat(compiler): Add an implementation for XHR that uses a template ca… · angular/angular@a596b88](https://github.com/angular/angular/commit/a596b88)

コンポーネントの`templateUrl`で指定されたテンプレートを、コンポーネントのロードより先に読み込んでキャッシュしておくための機能が実装されました。
karmaなどのテスト時にネックとなるファイル読み込みのXHRの発生を抑制し、`templateUrl`でテンプレートを読み込むコンポーネントを同期的に処理できます。
ただし、まだキャッシュをセットする側の機構が貧弱なのでアプリケーションサイドでは使いづらいです。
アプリケーションランドでのプリキャッシュ機構は将来的に実装するらしいので、それを待ちましょう。

テンプレートのキャッシュには`CACHED_TEMPLATE_PROVIDER`と`CachedXHR`、そして`window.$templateCache`を使います。

**CACHED_TEMPLATE_PROVIDER**:

後述の`CachedXHR`をInjectするためのProviderです。 `angular2/platform/browser`でexportされています。

**CachedXHR**:

内部にキャッシュを持ち、実際にはXHRを行わずにすべてをキャッシュから返すクラスです。

ただしキャッシュをセットすることは`CachedXHR`では担っておらず、次の`$templateCache`が実際にテンプレートのキャッシュを管理しています。

**window.$templateCache**:

windowに生やすグローバル変数です。次のように使います。

```ts
function setTemplateCache(cache): void {
  (<any>window).$templateCache = cache;
}

function createCachedXHR(): CachedXHR {
  setTemplateCache({'test.html': '<div>Hello</div>'});
  return new CachedXHR();
}
```

テスト中での具体的な使用例は次のコードを読むと参考になるでしょう

[angular/xhr_cache_spec.ts at master · angular/angular](https://github.com/angular/angular/blob/master/modules/angular2/test/platform/browser/xhr_cache_spec.ts)

### ngForのローカル変数に`first`が追加された

[feat(ngFor): Support convenience view local in ngFor · angular/angular@ccff175](https://github.com/angular/angular/commit/ccff175)

`ngFor`が提供する変数に、ループの1件目かどうかを表す`first`が追加されました。

```html
<div *ngFor="#item of items; #isFirst=first">{{isFirst.toString()}}</div>
```

余り知られていない気がしますが、`last`とか`index`とか`even`とか`odd`とか元々あります。

### オフラインコンパイル
まだユーザーランドには降りてきていないですが、内部的にはオフラインコンパイルが可能な機構になってきています。
おそらくangular-cliとcodelyzerが近日中に対応するので、その様子を見て使い方を勉強できそうです。


### その他
ngUpgrade側で`ngOnInit`の連携がうまくいってなかったバグなどいくつかが修正されてます。
詳しくはチェンジログを参照してください

----

今回はいくつかのサイトや記事も紹介します。

### 5thingsAngular
[5thingsAngular - Get weekly updates on 5 things Angular](http://5thingsangular.github.io/)

thoughtram blogでお馴染みのPascal Prechtが個人で始めたサイトです。
毎週月曜日(日本時間で火曜日)に、一週間の間に起きたAngular関連のトピックを5つ紹介してくれます。
RSSやメール購読など可能なので、ぜひチェックしてみてください。

### 5 Rookie Mistakes to Avoid with Angular 2
[5 Rookie Mistakes to Avoid with Angular 2](http://angularjs.blogspot.jp/2016/04/5-rookie-mistakes-to-avoid-with-angular.html)

GooglerのKara EricksonがAngular公式ブログに書いた記事です。
初心者が陥る5つのミスについて書かれています。

--- 

最後に私のイベントの宣伝です。

### ng-sake #2
[ng-sake #2 - connpass](http://ng-sake.connpass.com/event/29591/)

酒好きのためのAngularミートアップ、ng-sakeのチケットがまだ余っています。
来週の水曜日なのでぜひいらしてください。

### ng-conf extended Tokyo
[ng-conf extended Tokyo - connpass](http://connpass.com/event/29136/)

ng-confをライブビューイングする会です。全然集まりませんが当日参加でもいいのでゴールデンウィークの予定が無い方はぜひ遊びに来てください

---

土曜日には大阪でAngular 2ハンズオンが開催されます。私もメンターとして参加しますので良いイベントになるよう努力します。

それでは。





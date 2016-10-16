+++
date = "2016-10-16T19:39:11+09:00"
title = "Angular 2.1.0のリリース"
tags = ["release"]

+++

こんにちは、らこです。Angular 2.1.0がリリースされました。正式リリース後はじめてのマイナーアップデートです。
マイナーアップデートまでにはベータリリースと、RCリリースを挟んでいるので、ここでは2.0.1からの重要な変更点をまとめて紹介します。

----

### [2.0.2](https://github.com/angular/angular/compare/2.0.1...2.0.2) 

#### Bug Fixes

* **compiler:** do not embed templateUrl in view factories in non-debug mode. ([#11818](https://github.com/angular/angular/issues/11818)) ([51e1994](https://github.com/angular/angular/commit/51e1994)), closes [#11117](https://github.com/angular/angular/issues/11117)

`angularCompilerOptions` の `debug` がtrueに設定されていないときはコンパイル後のngfactoryファイルに `templateUrl` が残らなくなりました

* **compiler:** fix `:host(tag)` and `:host-context(tag)` ([a6bb84e0](https://github.com/angular/angular/commit/a6bb84e02b7579f8d957ef6ba5b10d83482ed756)), closes [#11972](https://github.com/angular/angular/issues/11972)
* **compiler:** fix attribute selectors in :host and :host-context ([#12056](https://github.com/angular/angular/issues/12056)) ([6f7ed32](https://github.com/angular/angular/commit/6f7ed32)), closes [#11917](https://github.com/angular/angular/issues/11917)
* **compiler:** support `@page` and `@document` CSS rules ([#11878](https://github.com/angular/angular/issues/11878)) ([c99ef49](https://github.com/angular/angular/commit/c99ef49)), closes [#11860](https://github.com/angular/angular/issues/11860)
* **compiler:** support `[attr="value with space"]` ([bd012ef](https://github.com/angular/angular/commit/bd012ef)), closes [#6249](https://github.com/angular/angular/issues/6249)
* **compiler:** support quoted attribute values ([7395400](https://github.com/angular/angular/commit/7395400)), closes [#6085](https://github.com/angular/angular/issues/6085)

コンポーネントのスタイルに関するバグ修正がいくつか入っています。
また、コンポーネントのスタイル中で `@page` と `@document` が使えるようになりました

* **forms:** properly validate empty strings with patterns ([#11450](https://github.com/angular/angular/issues/11450)) ([e00de0c](https://github.com/angular/angular/commit/e00de0c))

`pattern`バリデーターで空文字列を許可しない正規表現だった場合に空文字列がinvalidになっていなかったバグが修正されました。

```ts
it('should error on failure to match empty string', () => {
    expect(Validators.pattern('[a-zA-Z]+')(new FormControl(''))).toEqual({
         'pattern': {'requiredPattern': '^[a-zA-Z]+$', 'actualValue': ''}
    });
});
```

* **http:** remove url params if provided value is null or undefined ([#11990](https://github.com/angular/angular/issues/11990)) ([9cc0a4e](https://github.com/angular/angular/commit/9cc0a4e))

`UrlSearchParams`クラスの`set`メソッドや`.append`メソッドで、値がnullあるいはundefinedだった時のふるまいが変わりました。

- `set`: 与えられたキーのパラメータを削除
- `append`: 何もしない

あっさりバグフィックスとして変更されてますが、地味に大きな変更なのでクエリパラメータの組み立てしている部分を見直したほうがいいかもしれません。

### [2.1.0-beta.0](https://github.com/angular/angular/compare/2.0.0...2.1.0-beta.0)

#### Features

* **router:** add router preloader to optimistically preload routes ([5a84982](https://github.com/angular/angular/commit/5a84982))

routerパッケージにPreloading機能が実装されました。Preloadingについては[前回の記事]()で軽く紹介しています。
また、Victor Savkin氏のブログでも詳しく解説されています。

https://vsavkin.com/angular-router-preloading-modules-ba3c75e424cb


### [2.1.0-rc.0](https://github.com/angular/angular/compare/2.1.0-beta.0...2.1.0-rc.0)

#### Features

* **animations:** provide aliases for `:enter` and `:leave` transitions ([#11991](https://github.com/angular/angular/issues/11991)) ([e884f48](https://github.com/angular/angular/commit/e884f48))

Animation DSLに `:enter`エイリアスと`:leave`エイリアスが実装されました。それぞれ`void => *`と`* => void`に対応したエイリアスです。

### [2.1.0 incremental-metamorphosis](https://github.com/angular/angular/compare/2.1.0-rc.0...2.1.0)


#### Bug Fixes

* **compiler:** validate `[@HostBinding](https://github.com/HostBinding)` name ([#12139](https://github.com/angular/angular/issues/12139)) ([13ecc14](https://github.com/angular/angular/commit/13ecc14))

`@HostBinding`に渡されるバインド対象のプロパティ名にバリデーションが入ります。`@HostBinding`に`[prop]`や`(event)`のような記号付きのターゲットが指定できなくなりました。

* **compiler-cli:** remove peerDependency on [@angular](https://github.com/angular)/platform-server ([#12122](https://github.com/angular/angular/issues/12122)) ([71b7654](https://github.com/angular/angular/commit/71b7654))

compiler-cliパッケージのpeerDependenciesからplatform-serverパッケージの記述が消えました。

* **forms:** allow optional fields with pattern and minlength validators ([#12147](https://github.com/angular/angular/issues/12147)) ([d22eeb7](https://github.com/angular/angular/commit/d22eeb7))
* **forms:** properly validate blank strings with minlength ([#12091](https://github.com/angular/angular/issues/12091)) ([f50c1da](https://github.com/angular/angular/commit/f50c1da))

`minlength`バリデーターの挙動が修正され、空文字列をinvalidとしなくなりました。空文字列を検出するためにはrequiredバリデーターと併用します。

* **http:** fix Headers initialization from Headers and Object ([#12106](https://github.com/angular/angular/issues/12106)) ([f4566f8](https://github.com/angular/angular/commit/f4566f8))

* **platform-browser-dynamic:** mark platformBrowserDynamic as stable API ([#12154](https://github.com/angular/angular/issues/12154)) ([bcef5ef](https://github.com/angular/angular/commit/bcef5ef))

platform-browser-dynamicパッケージの`platformBrowserDynamic()`関数に_stable_ラベルが付けられました。

* **router:** parent resolve should complete before merging resolved data ([1681e4f](https://github.com/angular/angular/commit/1681e4f)), closes [#12032](https://github.com/angular/angular/issues/12032)

入れ子になったルートへのナビゲーション時に、親のResolveを待ってから子の遷移が行われるようになりました。

----

### 記事紹介

* [The Angular 2 revolution is here by Gerard Sans](http://slides.com/gerardsans/imworld-ng2-revolution#/)

gerardsans氏によるAngular 2の紹介スライドです。最新の内容を踏まえてわかりやすくまとめられているので見てみるとよいでしょう。

* [Angular Router: Understanding Redirects](https://vsavkin.com/angular-router-understanding-redirects-2826177761fc#.yqqlb97m6)

毎度のVictor Savkin氏のブログです。routerパッケージのリダイレクトのテクニックがまとめられています。

* [Versioning and Releasing Angular](http://angularjs.blogspot.jp/2016/10/versioning-and-releasing-angular.html?m=1)

Angular公式ブログより、今後のバージョニングとリリースに関する記事です。必読です。

* [Ahead\-of\-Time Compilation \- ts](https://angular.io/docs/ts/latest/cookbook/aot-compiler.html)

angular.ioにAoTコンパイルのクックブックが追加されました

* [Resolving route data in Angular 2 by thoughtram](http://blog.thoughtram.io/angular/2016/10/10/resolving-route-data-in-angular-2.html)

thoughtramブログより、routerパッケージのResolve機能に関する記事です

* [Two\-way Data Binding in Angular 2 by thoughtram](http://blog.thoughtram.io/angular/2016/10/13/two-way-data-binding-in-angular-2.html)

thoughtramブログよりもうひとつ。Angular 2の双方向データバインディングについてまとめられた記事です。

----

公式ブログにも書かれているように、今後は毎週新しいバージョンのリリースが続けられます。とはいえベータリリースすべてに追従する必要はないので、おちついて4週間ごとのマイナーアップデートに対応していけばよいでしょう。
それではまた次回のリリースで。



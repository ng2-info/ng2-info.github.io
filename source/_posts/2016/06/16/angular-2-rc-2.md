title: Angular 2 RC.2がリリースされました
date: 2016-06-16 20:51:37
author: laco0416
tags: 
- Release
- RC
---

どうも、らこです。ようやくRC.2がリリースされたので変更点を確認しましょう。
1ヶ月以上の間に多くの変更が盛り込まれたので数は多いですが、破壊的変更はほとんどないので、
落ち着いて移行していきましょう

[angular/CHANGELOG.md at master · angular/angular](https://github.com/angular/angular/blob/master/CHANGELOG.md#200-rc2-2016-06-15)

---

## Angular 2 RC.2 変更点

CHANGELOG.mdを見ればわかるようにとんでもない変更の数なので、分類してまとまりごとに見ていきましょう。
まず、アプリケーション開発に関係のあるものだけを抽出すると次のようになります

### Bug Fixes

* **bootstrap:** Swap coreBootstrap() and coreLoadAndBootstrap() arguments ([f95a604](https://github.com/angular/angular/commit/f95a604))
* **compiler:** Support for comment finishing with multiple dashes ([60a2ba8](https://github.com/angular/angular/commit/60a2ba8)), closes [#7119](https://github.com/angular/angular/issues/7119)
* **compiler:** have CSS parser support nested parentheses inside functions ([ceac045](https://github.com/angular/angular/commit/ceac045)), closes [#7580](https://github.com/angular/angular/issues/7580)
* **Control:** Support select multiple with Control class (#8069) ([84f859d](https://github.com/angular/angular/commit/84f859d))
* **facade:** change EventEmitter to be sync by default (#8761) ([e5904f4](https://github.com/angular/angular/commit/e5904f4))
* **forms:** rename old forms folder to forms-deprecated ([515a8e0](https://github.com/angular/angular/commit/515a8e0))
* **forms:** update accessor value when native select value changes ([7a2ce7f](https://github.com/angular/angular/commit/7a2ce7f)), closes [#8710](https://github.com/angular/angular/issues/8710)
* **forms:** update value and validity when controls are added ([50acb96](https://github.com/angular/angular/commit/50acb96)), closes [#8826](https://github.com/angular/angular/issues/8826)
* **forms:** separate ngModelGroup from formGroupName ([5c0cfde](https://github.com/angular/angular/commit/5c0cfdee48ba5aa48528a1c20ffd99318ee716ae))
* **http:** Set response.ok ([9234035](https://github.com/angular/angular/commit/9234035)), closes [#6390](https://github.com/angular/angular/issues/6390) [#6503](https://github.com/angular/angular/issues/6503)
* **metadata:** Allow spacing in multiple selectors (#7418) ([b2e804c](https://github.com/angular/angular/commit/b2e804c))
* **ngSwitch:** use switchCase instead of switchWhen (#9076) ([e1fcab7](https://github.com/angular/angular/commit/e1fcab7))
* **Request:** Change Request.text's return type to string ([b2e0946](https://github.com/angular/angular/commit/b2e0946)), closes [#8138](https://github.com/angular/angular/issues/8138)
* **router:** Added pushState fallback for IE 9 browser. ([bab6023](https://github.com/angular/angular/commit/bab6023)), closes [#6506](https://github.com/angular/angular/issues/6506) [#7929](https://github.com/angular/angular/issues/7929)
* **testing:**  add discardPeriodicTasks to be used with fakeAsync (#8629) ([0cb93a4](https://github.com/angular/angular/commit/0cb93a4)), closes [#8616](https://github.com/angular/angular/issues/8616)
* **upgrade:** allow deeper nesting of ng2 components/directives (#8949) ([48bf349](https://github.com/angular/angular/commit/48bf349))
* **upgrade:** allow functions for template and templateUrl (#9022) ([a19c4e8](https://github.com/angular/angular/commit/a19c4e8))
* **upgrade:** Ensure upgrade adapter works on angular.js 1.2 (#8647) ([cbc8d0a](https://github.com/angular/angular/commit/cbc8d0a))
* **upgrade:** fallback to root ng2 injector when element is compiled outside the document (#86 ([db82906](https://github.com/angular/angular/commit/db82906))
* **upgrade:** make bindings available on $scope in controller & link function (#8645) ([6cdc53c](https://github.com/angular/angular/commit/6cdc53c))

### Features

* **ChangeDetectorRef:** make detectChanges() correct ([6028368](https://github.com/angular/angular/commit/6028368)), closes [#8599](https://github.com/angular/angular/issues/8599)
* **common:** DatePipe supports ISO string ([abc266f](https://github.com/angular/angular/commit/abc266f)), closes [#7794](https://github.com/angular/angular/issues/7794)
* **common/datePipe:** change date formatter to use correct pattern closes #7008 (#8154) ([324f014](https://github.com/angular/angular/commit/324f014)), closes [#7008](https://github.com/angular/angular/issues/7008) [(#8154](https://github.com/(/issues/8154)
* **compiler:** Add support for `<ng-container>` ([0dbff55](https://github.com/angular/angular/commit/0dbff55bc6750653c5f8decc06d07e7269e3d6a5))
* **ComponentResolver:** Add a SystemJS resolver for compiled apps (#9145) ([a6e5ddc](https://github.com/angular/angular/commit/a6e5ddc))
* **core:** add a component resolver that can load components lazily using system.js ([1a0aea6](https://github.com/angular/angular/commit/1a0aea6))
* **core:** introduce support for animations ([5e0f8cf](https://github.com/angular/angular/commit/5e0f8cf)), closes [#8734](https://github.com/angular/angular/issues/8734)
* **core/linker:** add SimpleChanges type to lifecycle_hooks to simplify OnChanges signature ([0a872ff](https://github.com/angular/angular/commit/0a872ff)), closes [#8557](https://github.com/angular/angular/issues/8557)
* **debug:** collect styles and classes for the DebugElement ([155b882](https://github.com/angular/angular/commit/155b882))
* **enableDebugTools:** return ComponentRef ([4086b49](https://github.com/angular/angular/commit/4086b49))
* **forms:** add the submitted flag to NgForm and NgFormModel directives ([420e83a](https://github.com/angular/angular/commit/420e83a)), closes [#2960](https://github.com/angular/angular/issues/2960) [#7449](https://github.com/angular/angular/issues/7449)
* **forms:** allow ngModel to register with parent form ([4ed6cf7](https://github.com/angular/angular/commit/4ed6cf7))
* **forms:** compose validator fns automatically if arrays ([61960c5](https://github.com/angular/angular/commit/61960c51a3b21d1cfba523f53016f6284182d4e3))
* **forms:** support setting control name in ngModelOptions ([a191e96](https://github.com/angular/angular/commit/a191e9697c32062eda06cd1f1cfd856d89c16026))
* **forms:** add easy way to switch between forms modules ([22916bb](https://github.com/angular/angular/commit/22916bb5d1abf2818d7d8d99d39605af251f42e4))
* **http:** added withCredentials support ([95af14b](https://github.com/angular/angular/commit/95af14b)), closes [#7281](https://github.com/angular/angular/issues/7281) [#7281](https://github.com/angular/angular/issues/7281)
* **http:** automatically set request Content-Type header based on body type ([0f0a8ad](https://github.com/angular/angular/commit/0f0a8ad)), closes [#7310](https://github.com/angular/angular/issues/7310)
* **http:** implement Response.prototype.toString() to make for a nicer error message ([89f6108](https://github.com/angular/angular/commit/89f6108)), closes [#7511](https://github.com/angular/angular/issues/7511)
* **http:** set the statusText property from the XMLHttpRequest instance ([3019140](https://github.com/angular/angular/commit/3019140)), closes [#4162](https://github.com/angular/angular/issues/4162)
* **NgTemplateOutlet:** add context to NgTemplateOutlet ([164a091](https://github.com/angular/angular/commit/164a091)), closes [#9042](https://github.com/angular/angular/issues/9042)
* **NgZone:** isStable ([587c119](https://github.com/angular/angular/commit/587c119)), closes [#8108](https://github.com/angular/angular/issues/8108)
* **security:** allow data: URLs for images and videos. ([dd50124](https://github.com/angular/angular/commit/dd50124))
* **security:** allow url(...) style values. ([15ae710](https://github.com/angular/angular/commit/15ae710)), closes [#8514](https://github.com/angular/angular/issues/8514)
* **security:** Automatic XSRF handling. ([4d793c4](https://github.com/angular/angular/commit/4d793c4))
* add minified bundles ([9175a04](https://github.com/angular/angular/commit/9175a04))
* **security:** expose the safe value types. ([50c9bed](https://github.com/angular/angular/commit/50c9bed)), closes [#8568](https://github.com/angular/angular/issues/8568)
* **security:** support transform CSS functions for sanitization. ([8b1b427](https://github.com/angular/angular/commit/8b1b427)), closes [#8514](https://github.com/angular/angular/issues/8514)
* **security:** warn users when sanitizing in dev mode. ([3e68b7e](https://github.com/angular/angular/commit/3e68b7e)), closes [#8522](https://github.com/angular/angular/issues/8522)
* **shadow_css:** add encapsulation support for CSS @supports at-rule ([cb84cbf](https://github.com/angular/angular/commit/cb84cbf)), closes [#7944](https://github.com/angular/angular/issues/7944)
* **ViewEncapsulation:** default ViewEncapsulation to configurable ([f93512b](https://github.com/angular/angular/commit/f93512b)), closes [#7883](https://github.com/angular/angular/issues/7883)

だいぶ絞りましたがまだ多いですね。1件ずつ見ていくとキリがないのでグループごとにまとめていきます

----

### コア機能関連

* **facade:** change EventEmitter to be sync by default (#8761) ([e5904f4](https://github.com/angular/angular/commit/e5904f4))

`EventEmitter` によるイベントの伝播が、デフォルトで同期処理になりました。
標準のHTML要素からのイベントと同じ振る舞いをするようになり、`@Output` との区別をしなくてよくなりました。

* **NgZone:** isStable ([587c119](https://github.com/angular/angular/commit/587c119)), closes [#8108](https://github.com/angular/angular/issues/8108)

`NgZone` が `isStable` プロパティを持ちます

### コンポーネント関連

* **compiler:** Support for comment finishing with multiple dashes ([60a2ba8](https://github.com/angular/angular/commit/60a2ba8)), closes [#7119](https://github.com/angular/angular/issues/7119)

`<!-- -->` のように複数のダッシュ記号で閉じられたコメントが許容されるようになりました

* **compiler:** have CSS parser support nested parentheses inside functions ([ceac045](https://github.com/angular/angular/commit/ceac045)), closes [#7580](https://github.com/angular/angular/issues/7580)

コンポーネントが読み込むCSS内で入れ子の関数が許可されました

* **ChangeDetectorRef:** make detectChanges() correct ([6028368](https://github.com/angular/angular/commit/6028368)), closes [#8599](https://github.com/angular/angular/issues/8599)

私のPRです。
`ChangeDetectorRef#detectChanges()` が、 `detach` 状態でも動作するように修正されました

* **compiler:** Add support for `<ng-container>` ([0dbff55](https://github.com/angular/angular/commit/0dbff55bc6750653c5f8decc06d07e7269e3d6a5))

コンパイル後にHTMLコメントになりDOMに影響しない `<ng-container>` が導入されました。
テンプレート内で階層構造だけを作りたいときに使えます。例えば `ngSwitch` で無駄なdivタグを作らなくてよくなります。

* **core/linker:** add SimpleChanges type to lifecycle_hooks to simplify OnChanges signature ([0a872ff](https://github.com/angular/angular/commit/0a872ff)), closes [#8557](https://github.com/angular/angular/issues/8557)

`ngOnChanges` メソッドに渡される引数の型が `SimpleChanges` 型になりました。
実体は`{[key:string]: SimpleChange}` のタイプエイリアスです

* **metadata:** Allow spacing in multiple selectors (#7418) ([b2e804c](https://github.com/angular/angular/commit/b2e804c))

これも私のPRです。
`@ViewChildren` や `@ContentChildren` に文字列でテンプレート変数を渡すときにカンマ区切りの中にスペースを許容するようになりました

* **shadow_css:** add encapsulation support for CSS @supports at-rule ([cb84cbf](https://github.com/angular/angular/commit/cb84cbf)), closes [#7944](https://github.com/angular/angular/issues/7944)

`ViewEncapusulation.Emulated` がCSSの `@supports` に対応しました。 

* **ViewEncapsulation:** default ViewEncapsulation to configurable ([f93512b](https://github.com/angular/angular/commit/f93512b)), closes [#7883](https://github.com/angular/angular/issues/7883)

これも私のPRです。
`Component` デコレータに `encapsulation` プロパティを設定しなかった時のデフォルトの設定を変更できるようになりました。
`CompilerConfig` から変更可能です

* **ComponentResolver:** Add a SystemJS resolver for compiled apps (#9145) ([a6e5ddc](https://github.com/angular/angular/commit/a6e5ddc))
* **core:** add a component resolver that can load components lazily using system.js ([1a0aea6](https://github.com/angular/angular/commit/1a0aea6))

SystemJSを使ったダイナミックなコンポーネントの読み込みをサポートするための仕組みが整いつつあります。

### HTTP関連

* **http:** Set response.ok ([9234035](https://github.com/angular/angular/commit/9234035)), closes [#6390](https://github.com/angular/angular/issues/6390) [#6503](https://github.com/angular/angular/issues/6503)
* **Request:** Change Request.text's return type to string ([b2e0946](https://github.com/angular/angular/commit/b2e0946)), closes [#8138](https://github.com/angular/angular/issues/8138)
* **http:** added withCredentials support ([95af14b](https://github.com/angular/angular/commit/95af14b)), closes [#7281](https://github.com/angular/angular/issues/7281) [#7281](https://github.com/angular/angular/issues/7281)
* **http:** automatically set request Content-Type header based on body type ([0f0a8ad](https://github.com/angular/angular/commit/0f0a8ad)), closes [#7310](https://github.com/angular/angular/issues/7310)
* **http:** implement Response.prototype.toString() to make for a nicer error message ([89f6108](https://github.com/angular/angular/commit/89f6108)), closes [#7511](https://github.com/angular/angular/issues/7511)
* **http:** set the statusText property from the XMLHttpRequest instance ([3019140](https://github.com/angular/angular/commit/3019140)), closes [#4162](https://github.com/angular/angular/issues/4162)

`RequestOptions` と `BaseRequestOptions` に `withCredentials` プロパティが追加されたことと、
`post()` や `put()` などの第2引数 `body` がany型となり、その型によって自動的にContent-Typeを設定してくれるようになったのが重要です。
今までワークアラウンドで解決していた部分が不要になるでしょう。

### ルーター関連
Router v3が出ていますのでRC.2に含まれているルーター関連のコミットはほとんど意味ないです。

* **router:** Added pushState fallback for IE 9 browser. ([bab6023](https://github.com/angular/angular/commit/bab6023)), closes [#6506](https://github.com/angular/angular/issues/6506) [#7929](https://github.com/angular/angular/issues/7929)

ブラウザがPushStateに対応していない時のフォールバックが追加されました

### パイプ・ディレクティブ関連

* **ngSwitch:** use switchCase instead of switchWhen (#9076) ([e1fcab7](https://github.com/angular/angular/commit/e1fcab7))

`ngSwitchWhen` が `ngSwitchCase` に改名されました。JavaScriptのswitch文と合わせた形です。

* **common:** DatePipe supports ISO string ([abc266f](https://github.com/angular/angular/commit/abc266f)), closes [#7794](https://github.com/angular/angular/issues/7794)

これも私のPRです。
DatePipeが `Date.fromISOString` で受け入れ可能な ISO形式の文字列を許容するようになりました

* **common/datePipe:** change date formatter to use correct pattern closes #7008 (#8154) ([324f014](https://github.com/angular/angular/commit/324f014)), closes [#7008](https://github.com/angular/angular/issues/7008) [(#8154](https://github.com/(/issues/8154)

DatePipeに渡すフォーマットが正しく動作するようになりました

* **NgTemplateOutlet:** add context to NgTemplateOutlet ([164a091](https://github.com/angular/angular/commit/164a091)), closes [#9042](https://github.com/angular/angular/issues/9042)

`ngTemplateOutlet` にcontext を渡せるようになりました

```html
<template [ngTemplateOutlet]="templateRefExpression" [ngOutletContext]="objectExpression"></template>
```

### Forms関連

* **Control:** Support select multiple with Control class (#8069) ([84f859d](https://github.com/angular/angular/commit/84f859d))
* **facade:** change EventEmitter to be sync by default (#8761) ([e5904f4](https://github.com/angular/angular/commit/e5904f4))
* **forms:** rename old forms folder to forms-deprecated ([515a8e0](https://github.com/angular/angular/commit/515a8e0))
* **forms:** update accessor value when native select value changes ([7a2ce7f](https://github.com/angular/angular/commit/7a2ce7f)), closes [#8710](https://github.com/angular/angular/issues/8710)
* **forms:** update value and validity when controls are added ([50acb96](https://github.com/angular/angular/commit/50acb96)), closes [#8826](https://github.com/angular/angular/issues/8826)
* **forms:** separate ngModelGroup from formGroupName ([5c0cfde](https://github.com/angular/angular/commit/5c0cfdee48ba5aa48528a1c20ffd99318ee716ae))
* **forms:** add the submitted flag to NgForm and NgFormModel directives ([420e83a](https://github.com/angular/angular/commit/420e83a)), closes [#2960](https://github.com/angular/angular/issues/2960) [#7449](https://github.com/angular/angular/issues/7449)
* **forms:** allow ngModel to register with parent form ([4ed6cf7](https://github.com/angular/angular/commit/4ed6cf7))
* **forms:** compose validator fns automatically if arrays ([61960c5](https://github.com/angular/angular/commit/61960c51a3b21d1cfba523f53016f6284182d4e3))
* **forms:** support setting control name in ngModelOptions ([a191e96](https://github.com/angular/angular/commit/a191e9697c32062eda06cd1f1cfd856d89c16026))
* **forms:** add easy way to switch between forms modules ([22916bb](https://github.com/angular/angular/commit/22916bb5d1abf2818d7d8d99d39605af251f42e4))

`@angular/common` の中の Forms APIはすべて deprecatedになりました。
`ngForm` や `ngModel` などの基本的な機能はデフォルトで読み込まれるので気にする必要はありませんが、
`FormBuilder` などのモデルドリブンフォームのAPIは `@angular/forms` から読み込むようになっていきます。

機能的には、select要素がmultipleをサポートできるようになったことや、
`ngModel` を設定している時に `ngControl` を使わなくてよくなりました。
また、 ngForm や ngFormModelが submitted フラグを持つようになりました。

### テスト・デバッグ関連

* **enableDebugTools:** return ComponentRef ([4086b49](https://github.com/angular/angular/commit/4086b49))

`enableDebugTools` が `ComponentRef` を返すようになったので、 `bootstrap()`の戻り値のPromiseの中で使いやすくなりました。

```ts
bootstrap(App)
  .then(enableDebugTools);
  .then(cmpRef => {
      ...
  });
```

* **testing:**  add discardPeriodicTasks to be used with fakeAsync (#8629) ([0cb93a4](https://github.com/angular/angular/commit/0cb93a4)), closes [#8616](https://github.com/angular/angular/issues/8616)

`discardPeriodicTasks` 関数が追加されました。 `fakeAsync` 中で残っているピリオディックなタスクを破棄します。

*  **debug:** collect styles and classes for the DebugElement ([155b882](https://github.com/angular/angular/commit/155b882))

`DebugElement` がstylesとclasssを持つようになりました。

### セキュリティ関連

* **security:** allow data: URLs for images and videos. ([dd50124](https://github.com/angular/angular/commit/dd50124))
* **security:** allow url(...) style values. ([15ae710](https://github.com/angular/angular/commit/15ae710)), closes [#8514](https://github.com/angular/angular/issues/8514)
* **security:** Automatic XSRF handling. ([4d793c4](https://github.com/angular/angular/commit/4d793c4))
* **security:** expose the safe value types. ([50c9bed](https://github.com/angular/angular/commit/50c9bed)), closes [#8568](https://github.com/angular/angular/issues/8568)
* **security:** support transform CSS functions for sanitization. ([8b1b427](https://github.com/angular/angular/commit/8b1b427)), closes [#8514](https://github.com/angular/angular/issues/8514)
* **security:** warn users when sanitizing in dev mode. ([3e68b7e](https://github.com/angular/angular/commit/3e68b7e)), closes [#8522](https://github.com/angular/angular/issues/8522)

ようやく落ち着きを見せてきたセキュリティ対応です。
`data:image/png` のような値が許可されたり、スタイルの `url()` 関数が許可されたり、CSS中の関数のバリデーションが緩和されたりしています。
また、ProdModeじゃない時にはセキュリティに引っかかったときにwarningメッセージが出てくるようになりました

### アップグレード関連

* **upgrade:** allow deeper nesting of ng2 components/directives (#8949) ([48bf349](https://github.com/angular/angular/commit/48bf349))

ng2 > ng1 > ng2 のような複雑な入れ子構造で正しく動くようになります

* **upgrade:** allow functions for template and templateUrl (#9022) ([a19c4e8](https://github.com/angular/angular/commit/a19c4e8))

ng1側のディレクティブの `template` や `templateUrl` が関数の場合にその戻り値をテンプレートとして扱うようになりました 

* **upgrade:** Ensure upgrade adapter works on angular.js 1.2 (#8647) ([cbc8d0a](https://github.com/angular/angular/commit/cbc8d0a))
* **upgrade:** make bindings available on $scope in controller & link function (#8645) ([6cdc53c](https://github.com/angular/angular/commit/6cdc53c))

Angular 1.2に対応しました。 `bindToController` がないときにはディレクティブのcontroller関数やlink関数で `$scope` から
ng2 componentからのデータバインディングが可能になりました

* **upgrade:** fallback to root ng2 injector when element is compiled outside the document (#86 ([db82906](https://github.com/angular/angular/commit/db82906))

ui-routerのように、ディレクティブを外部でコンパイルしたものをドキュメントに挿入した時に、
挿入されたディレクティブがルートのInjectorを参照するようにフォールバックが追加されました


----

いかがでしたでしょうか。
大きく変わったのはForms周りくらいで、あとはちょっと便利な機能が増えたりバグが直ったりと嬉しい変更ばかりです。
新しいForms APIに関してはまだ未完成なので今後も要チェックです。しばらくは今までどおりのAPIを使っていてもいいでしょう。
RC.3は来月以降とのことなのでゆっくり慣らしていけるはずです。

Router v3も細かい修正が重なっているので、落ち着いたところでまた変更点を紹介します。
それではまた。
title: angular-2-rc-4
date: 2016-07-08 21:57:10
author:
tags:
---

こんにちは、らこです。RC.3に引き続きアップデートからしばらく経ってしまいましたが、変更点についてまとめておきます。
今回は破壊的な変更もそこそこあるのでしっかり把握しておきましょう。

----

[2.0.0-RC.4](https://github.com/angular/angular/blob/master/CHANGELOG.md#200-rc4-2016-06-30)

### 破壊的変更

#### テスト関連
テスト関連の変更が非常に多いのでざっくりまとめます。

**なくなったもの**

* `it`, `iit`, `xit`
* `describe`, `ddescribe`, `xdescribe`
* `expect`
* `toThrowErrorWith`
* `toMatchPattern`

Jasmineの関数をラップしていたものたち 

Before:

```ts
import {beforeEachProviders, it, describe, inject} from '@angular/core/testing';

describe('my code', () => {
    beforeEachProviders(() => [MyService]);

    it('does stuff', inject([MyService], (service) => {
      // actual test
    });
});
```

After:

```ts
import {addProviders, inject} from '@angular/core/testing';

describe('my code', () => {
    beforeEach(() => {
        addProviders([MyService]);
    });

    it('does stuff', inject([MyService], (service) => {
        // actual test
    });
});
```

* `MockLocationStrategy`
* `browserDetection`
* `dispatchEvent`
* `el`
* `normalizeCSS`
* `stringifyElement`

内部用APIに。Locationのテストは`SpyLocation`で。

* `injectAsync`
* `clearPendingTimers`
* `Log`
* `MockAppliacationRef`
* `MockNgZone`
* `clearPendingTimers`
* `getTypeOf`
* `instantiateType`

不要になったものたち

**変わったもの**

* `beforeEachProviders` => `addProviders` 

任意のタイミングで使う汎用のAPIとなった

* `TestComponentBuilder.createSync`

与えられたコンポーネントのテンプレートが事前にインラインにコンパイルされていなければエラーを出すようになった


**移動したもの**

* `TestComponentBuilder`
* `TestComponentRenderer`
* `ComponentFixture`
* `ComponentFixtureAutoDetect`

 `compiler/testing` => `core/testing` に移動しました。

#### その他
* httpモジュールの`URLSearchParams`を使った時に、クエリパラメータが誤ってエンコードされていたのを修正したため、
RC.4の前後でリクエストのクエリパラメータが変化することがあります。

RC.4からのデフォルトでは、クエリパラメータ中に含まれる`@ : $ , ; + ? /`記号はそのまま使用されます。
この挙動をカスタマイズするには、 `URLSearchParams`のコンストラクタの第2引数に`QueryEncoder`を継承したクラスのインスタンスを渡します。

```ts
import {URLSearchParams, QueryEncoder} from '@angular/http';

class MyQueryEncoder extends QueryEncoder {
    encodeKey(k: string): string {
        return myEncodingFunction(k);
    }
 
    encodeValue(v: string): string {
        return myEncodingFunction(v);
    }
}
let params = new URLSearchParams('', new MyQueryEncoder());
```

* RC.3以前は同じ要素に対して`*ngFor`と`*ngIf`のように`*`プレフィックスのディレクティブ(テンプレートバインディング)を複数付与することができていましたが、
今後はこれは禁止されます。
これまでは使用可能ではありましたが大抵の場合は想定外の結果を招いていました。
`*ngIf`と`*ngFor`が両方必要な場合には入れ子要素にするか、`*`プレフィックスを使わずに`<template>`タグを明示的に使う記法を選択してください。


* `DomEventsPlugin`と`KeyEventsPlugin`はこれまでパブリックなAPIとして公開されていましたが、非公開APIとなりました。
また、非推奨になっていた`BROWSER_PROVIDERS`は削除されました。

### アニメーション関連
アニメーション周りのバグ修正がいくつか入っています。

* **animations:** ensure void => * animations are triggered when an expression is omitted ([e0b0a59](https://github.com/angular/angular/commit/e0b0a59)), closes [#9327](https://github.com/angular/angular/issues/9327) [#9381](https://github.com/angular/angular/issues/9381)

`void => *`をトリガーとするアニメーションが条件により発火しないバグが修正されました 

* **animations:** make sure the easing value is passed into the web-animations player ([c43aec2](https://github.com/angular/angular/commit/c43aec2)), closes [#9517](https://github.com/angular/angular/issues/9517) [#9523](https://github.com/angular/angular/issues/9523)

`easing`オプションがアニメーションプレイヤーに渡されていなかったバグが修正されました。

### その他

* support *directive on `<template>` ([#9691](https://github.com/angular/angular/issues/9691)) ([3fec279](https://github.com/angular/angular/commit/3fec279)), closes [#7315](https://github.com/angular/angular/issues/7315)

`*`プレフィックスのディレクティブが`<template>`タグ上でも使えるようになりました。

* **core:** properly report missing providers and viewProviders ([#9411](https://github.com/angular/angular/issues/9411)) ([f114dd3](https://github.com/angular/angular/commit/f114dd3)), closes [#8237](https://github.com/angular/angular/issues/8237)

コンポーネントやサービスのインスタンス化において、解決できないDIのがあったときのエラーがわかりやすくなりました。

`One or more of providers for "MyBrokenComp3" were not defined: [?, SimpleService, ?].` のように、解決できなかった位置に?が表示されます。

* **forms:** add select multiple accessor as built-in accessor ([9f00a1b](https://github.com/angular/angular/commit/9f00a1b))

multipleなselect要素の値へのアクセスをデフォルトでサポートするようになりました。

* **forms:** emit statusChange when child controls have async validator ([#9652](https://github.com/angular/angular/issues/9652)) ([797914e](https://github.com/angular/angular/commit/797914e))

async validatorを使っているときに、バリデーション結果が変化したことを`statusChange`イベントで検知できるようになりました

* **forms:** make radio button selection logic more flexible ([#9646](https://github.com/angular/angular/issues/9646)) ([ed0ade6](https://github.com/angular/angular/commit/ed0ade6)), closes [#9558](https://github.com/angular/angular/issues/9558)

これまでラジオボタンに対する`removeControl`はtimeoutが必要でしたが、必要なくなりました。

* **forms:** ngModel should emit valueChanges and statusChanges asynchronously ([97a2119](https://github.com/angular/angular/commit/97a2119))

`ngModel` が`valueChanges`や`statusChanges`イベントを発火するタイミングが非同期的になりました。


### Features
機能の追加もいくつか行われています。

* **compiler:** support sync runtime compile ([bf598d6](https://github.com/angular/angular/commit/bf598d6)), closes [#7084](https://github.com/angular/angular/issues/7084) [#9594](https://github.com/angular/angular/issues/9594)

`ComponentResolver`に代わる新しいAPI、 `Compiler`が、`compileComponentAsync`と`compileComponentSync`の2つのメソッドを持つようになりました。
`compileComponentSync`はコンポーネントを同期的にコンパイルできますが、テンプレートがインライン、あるいはすでに読み込まれたものでなければなりません。

* **core:** add `[@Component](https://github.com/Component).precompile` and `ComponentFactoryResolver` ([6c5b653](https://github.com/angular/angular/commit/6c5b653)), closes [#9543](https://github.com/angular/angular/issues/9543)

`@Component`デコレータに新しいプロパティ`precompile`が追加されました。
このプロパティは、対象のコンポーネントがOffline Compileされる際に、一緒にコンパイルされて欲しいコンポーネントを指定できるものです。
今はまだドキュメントがないですが、RC.5が出てくる頃には用意されるでしょう。

* **forms:** add support for formArrayName ([c03e1f2](https://github.com/angular/angular/commit/c03e1f2)), closes [#9251](https://github.com/angular/angular/issues/9251)
* **forms:** add support for standalone ngModel dirs inside forms ([6edf047](https://github.com/angular/angular/commit/6edf047)), closes [#9230](https://github.com/angular/angular/issues/9230)
* **forms:** expose ValidatorFn and AsyncValidatorFn ([17dcbf6](https://github.com/angular/angular/commit/17dcbf6)), closes [#8834](https://github.com/angular/angular/issues/8834)
* **forms:** make valueChanges and statusChanges available on abstract control directives ([de12710](https://github.com/angular/angular/commit/de12710))
* **forms:** support updating of validators on exiting controls ([#9516](https://github.com/angular/angular/issues/9516)) ([638fd74](https://github.com/angular/angular/commit/638fd74))
* **forms:** use formControlName on radio buttons when name is absent ([#9681](https://github.com/angular/angular/issues/9681)) ([0961bd1](https://github.com/angular/angular/commit/0961bd1))

Forms周りは引き続き作業中という感じです。

* **QueryList:** implement some() ([#9464](https://github.com/angular/angular/issues/9464)) ([f6a410a](https://github.com/angular/angular/commit/f6a410a)), closes [#9443](https://github.com/angular/angular/issues/9443)

`QueryList#.some()`が実装されました。

* **router:** add pathMatch property to replace terminal ([fcfddbf](https://github.com/angular/angular/commit/fcfddbf))

RouteConfigの`terminal`が廃止され、`pathMatch`フィールドが追加されました。

`pathMatch: "full"`の場合は、パスと完全一致した時にだけそのルートが使われます。

* **router:** implement data and resolve ([f2f1ec0](https://github.com/angular/angular/commit/f2f1ec0))

ActivatedRouteに`data`が追加されました。また、`resolve`も追加されました。

dataは単純に固定の値をRouteConfig側で設定しておき、それがActivatedRouteからアクセスできるだけの機能です。
主に同じコンポーネントを複数のルートで使う時に、どのルートが使われているかの判別などに用います。

resolveはDIを使って、動的なデータをActivatedRouteから受け取るための機能です。
dataもresolveもまだ公式ドキュメントは追いついていないので、
[このあたり](https://github.com/angular/angular/blob/master/modules/%40angular/router/test/router.spec.ts#L418-L458)のテストコードを読むと使い方がわかるでしょう。


----


いやはや、今回も長かったですね。RC.5では`AppModule`が導入され、bootstrap周りに大きな動きが出る予定です。
`PLATFORM_DIRECTIVES`や`PLATFORM_PIPES`、さらには`APP_INITIALIZER`あたりが非推奨APIとなります。
デザインドキュメントは[こちら](https://docs.google.com/document/d/13-LUm1QvOff2631tHz6C4goIHuMzma2_1_PFiLryoIs/edit)にあるので、先取りしたい方は読んでおくとよいでしょう。
トラッキングイシューは[こちら](https://github.com/angular/angular/issues/9726)です。

それではまた。



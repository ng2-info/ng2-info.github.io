+++
date = "2016-09-24T06:29:45+09:00"
title = "Angular 2.0.1/2.1.0-beta.0のリリース"
tags = ["release"]

+++

どうも、らこです。Angular 2.0.0の公開から初めてのアップデートがリリースされました。
今回はバグ修正だけを取り込んだ2.0.1と、新しい機能を追加した2.1.0-beta.0の同時リリースです。2.0.0リリース時の宣言通り、semverに従ったバージョニングになっています。
それでは今回の変更点をピックアップしていきます。

<!--more-->

[2.0.1](https://github.com/angular/angular/blob/master/CHANGELOG.md#201-2016-09-23)
[2.1.0-beta.0](https://github.com/angular/angular/blob/master/CHANGELOG.md#210-beta0-2016-09-23)

## ベータ版のインストール方法

今回より、npmのリリースタグを用いて安定版とベータ版を同時リリースしています。`npm info @angular/core` コマンドを実行すればわかりますが、**latest**バージョンが2.0.1、**next**バージョンが2.1.0-beta.0になっています。
何もバージョンを指定せずにnpmでインストールするとlatestタグが使われます。2.1.0-beta.0をインストールする場合は、 `npm install @angular/core@next` というふうにタグを付けましょう。

## 2.0.1の修正点

* **common:** fix ngOnChanges signature of NgTemplateOutlet directive ([14ee759](https://github.com/angular/angular/commit/14ee759))

`NgTemplateOutlet`クラスの`ngOnChanges`メソッドが正しく宣言されていなかった問題が修正されました。
AoTコンパイル時に`NgTemplateOutlet`を使うとエラーが発生していたのが解決されます。

* **compiler:** `[attribute~=value]` selector ([#11696](https://github.com/angular/angular/issues/11696)) ([734b8b8](https://github.com/angular/angular/commit/734b8b8)), closes [#9644](https://github.com/angular/angular/issues/9644)

`[attribute~=value]`形式のセレクターを使ったCSSが、正しくShadow CSS化されない問題が修正されました。

* **compiler:** safe property access expressions work in event bindings ([#11724](https://github.com/angular/angular/issues/11724)) ([a95d652](https://github.com/angular/angular/commit/a95d652))

`(click)="foo?.bar()"` というふうに、イベントバインディングの文の中で `?.` シンタックスが使えるようになりました。

* **compiler:** throw when Component.moduleId is not a string ([bd4045b](https://github.com/angular/angular/commit/bd4045b)), closes [#11590](https://github.com/angular/angular/issues/11590)

`Component.moduleId`について、実行時に文字列型かどうかのチェックを行うようになりました。

* **compiler:** do not provide I18N values when they're not specified ([03aedbe](https://github.com/angular/angular/commit/03aedbe)), closes [#11643](https://github.com/angular/angular/issues/11643)

AoTコンパイル時に、常に`--locale`オプションが求められていた不具合が修正されました。

* **core:** ContentChild descendants should be queried by default ([0dc15eb](https://github.com/angular/angular/commit/0dc15eb)), closes [#1645](https://github.com/angular/angular/issues/1645)

`ContentChild`が、直下の要素だけでなくその子孫までクエリするのをデフォルトの動作としました。
次のような場合に、`<some-component>` は `#foo` で参照される要素をクエリするようになります。

```html
<some-component>
    <div>
        <div #q></div>
    </div>
</some-component>
```

* **forms:** disable all radios with disable() ([2860418](https://github.com/angular/angular/commit/2860418))

ラジオボタンのコントロールに対して`disable`メソッドを呼び出したときに、グループ内のすべてのラジオボタンを無効にするよう修正されました。

* **forms:** make setDisabledState optional for reactive form directives ([#11731](https://github.com/angular/angular/issues/11731)) ([51d73d3](https://github.com/angular/angular/commit/51d73d3)), closes [#11719](https://github.com/angular/angular/issues/11719)

`ControlValueAccessor#setDisabledState`メソッドがオプショナルにもかかわらず常に呼び出されていた部分が修正されました。

* **forms:** support unbound disabled in ngModel ([#11736](https://github.com/angular/angular/issues/11736)) ([39e251e](https://github.com/angular/angular/commit/39e251e))

`ngModel`ディレクティブの`disabled`プロパティを使いやすくする修正が行われました。

* **upgrade:** allow attribute selectors for components in ng2 which are not part of upgrade ([#11808](https://github.com/angular/angular/issues/11808)) ([b81e2e7](https://github.com/angular/angular/commit/b81e2e7)), closes [#11280](https://github.com/angular/angular/issues/11280)

`UpgradeAdapter`を使った際に、Angular 2側のテンプレートでは属性セレクタのコンポーネントを使用できるようになりました。

## 2.1.0-beta.0の変更点

### Features

* **router:** add router preloader to optimistically preload routes ([5a84982](https://github.com/angular/angular/commit/5a84982))

routerパッケージに、**Preloading**という仕組みが導入されます。これはrouterのLazyLoading機能によってAngularモジュールの遅延読み込みを行うとき、モジュールの読み込みのタイミングを制御するためのものです。
Preloadingの処理は`PreloadingStrategy`という設定によって制御され、デフォルトでは全くPreloadingを行わない`NoPreloading`が設定されています。
ビルトインの`PreloadingStrategy`は、`NoPreloading`の他にもうひとつ`PreloadAllModules`があり、こちらは逆にすべての子モジュールを即座に読み込む設定です。
Preloadingは、`canLoad`が設定されているルートとその子孫には適用されません。

`PreloadingStrategy`の設定は、`RouterModule.forRoot`メソッドの第二引数オプションに次のように渡します。

```ts
RouteModule.forRoot(ROUTES, {preloadingStrategy: PreloadAllModules})
```

### Bug Fixes
* **router:** update the router not to reset router state when updating root component ([#11799](https://github.com/angular/angular/issues/11799)) ([31dce72](https://github.com/angular/angular/commit/31dce72))

routerパッケージの次期バージョン 4.0.0に向けた内部の修正です

----

## 記事・動画紹介

* [The Powerful URL Matching Engine of Angular Router](https://vsavkin.com/the-powerful-url-matching-engine-of-angular-router-775dad593b03#.899t2sn9b)

Victor Savkin氏がRouterのURLルーティングの規則や機能などを詳しく解説しています。

* [Angular 2 Animations \- Foundation Concepts](http://blog.thoughtram.io/angular/2016/09/16/angular-2-animation-important-concepts.html)

安定と信頼のthoughtram blogです。Angular 2のアニメーションAPIについて概要の解説をしています。

* [Webinar: using new ngModule in Angular 2 w/ Pascal Precht \(LIVE\)](https://www.youtube.com/watch?v=Usohbij6frA&feature=em-lss)

Pascal Precht氏による`NgModule`機能のwebinarです。とてもわかりやすく重要なポイントを解説しているので必見です。

----

今回はこのあたりで。それではまた。



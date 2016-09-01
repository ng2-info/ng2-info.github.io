+++
date = "2016-09-01T21:23:17+09:00"
tags = ["Release", "RC"]
title = "Angular 2 RC.6がリリースされました"

+++

どうも、らこです。Angular 2のRC.6がリリースされました。
RC.6はFinalリリースへの最後の一歩です。
非推奨だったAPIがきれいに削除され、RC.5で導入されたNgModuleを基本としたAPIに整えられました。
RC.5でAngularモジュール(`NgModule`)対応が済んでいないと、RC.6にアップデートできません。
まずはRC.5で準備を整えてから、最終リリースへの階段を登りましょう。

それでは今回の重要な変更点をピックアップしていきます。

<!--more-->

[2.0.0-RC.6](https://github.com/angular/angular/blob/master/CHANGELOG.md#200-rc6-2016-08-31)

## 破壊的変更

まずはRC.6の破壊的変更から。破壊的変更とはいいますが、ほとんどはRC.5時点で非推奨となったAPIの完全な削除です。

### npmパッケージの構成の変更

RC.6から、npmパッケージとして配布されるAngular 2のソースコードは、 **ESM** (ES6 Modules)形式のJavaScriptになりました。
これはRollupや、Webpack2などの、Tree Shakingに対応したbundlerのための変更です。
これまでどおりCommonJS形式でAngular 2を読み込む場合は、 `bundles/*.umd.js` にあるUMD形式のファイルを使う必要があります。
ただし、ほとんどの場合はpackage.jsonの`main`プロパティによって自動的にそちらに誘導されるので、特に設定を変える必要はありません。

SystemJSに関しては、[こちらの設定](https://github.com/angular/quickstart/blob/3b7452cc444c49c139ea39523ced0468c2362c16/systemjs.config.js#L17-L34)を参考にするとよいでしょう。

### zone.jsのアップデートにともなうtestingへの影響

依存するzone.jsのバージョンが上がったことにともない、ユニットテスト環境でのzone.jsの読み込みに変更があります。
詳細は [こちら](https://github.com/angular/quickstart/blob/3b7452cc444c49c139ea39523ced0468c2362c16/karma.conf.js#L31-L38)を参考にしてください。

なお、今回の変更により **Jasmine** 以外のテスティングフレームワークを使おうとするとエラーが発生する問題が起きています。

[RC6 unit tests with mocha fail to run · Issue \#11230 · angular/angular](https://github.com/angular/angular/issues/11230)

**mocha**などのフレームワークを使っている場合は、今のところはRC.6へのアップデートは避けたほうがよいでしょう。
上記Issueをウォッチしておくことをおすすめします。

### `SanitizationService`/`DomSanitizationService`の改名

上記APIはそれぞれ、 `Sanitizer`/`DomSanitizer`に名前が変更されました。
名前以外の変更はありません。

### `@Component.directives/pipes`の完全廃止

`@Component`デコレーターが受け取るメタデータについて、
`directives`プロパティと、`pipes`プロパティが完全に削除されました。
RC.5で導入された`@NgModule`デコレーターの`declarations`を使用してください。

また、この変更にともなって、Component単位でのコンパイルも廃止されています。
具体的には以下のAPIが廃止されています

- `DynamicComponentLoader`
- `ComponentResolver`
- `Compiler.compileComponentAsync/Sync`
- `SystemJsComponentResolver`/`SystemJsCmpFactoryResolver`

動的なコンポーネント作成の際は、`ComponentFactoryResolver`から該当のコンポーネントの`ComponentFactory`を取得してください。

### アニメーション定義の非推奨なテンプレートシンタックスを廃止

RC.5で追加された新しいシンタックスに移行し、古いシンタックスは廃止されます。

```html
<!-- this is now invalid -->
<div @flip="flipState"></div>

<!-- change that to -->
<div [@flip]="flipState"></div>
```

`@`プレフィックスではなく`animate-`プレフィックスを使う場合も同様で、`bind-`プレフィックスが必須になります。

```html
<!-- this is now invalid -->
<div animate-flip="flipState"></div>

<!-- is valid now -->
<div bind-animate-flip="flipState"></div>
```

### 非推奨なプロバイダー宣言を廃止

これまで非推奨だったプロバイダーの宣言方法が廃止され、ひとつの書き方に統一されます。

```ts
// NG
bind(MyClass).toFactory(...)
new Provider(MyClass, toFactory: ...)

// OK
{provider: MyClass, toFactory: ...}
```

### `NgZoneError`の廃止

非推奨にされていた`NgZoneError`が削除されました。

### 非推奨なboostrap APIの廃止

`coreBootstrap`と`coreLoadAndBootstrap`が廃止されました

### `ApplicationRef`,`PlatformRef`の非推奨APIの廃止

`ApplicationRef`と`PlatformRef`関連のAPIで非推奨なものがすべて廃止されました

### 非推奨な`@Directive.properties/events`の廃止

非推奨なAPIだった`@Directive`や`@Component`デコレーターの`properties`プロパティと`events`プロパティが廃止されました。
これらは`inputs`と`outputs`を使用します。

### `Query`/`ViewQuery`の廃止

非推奨でした(略)

### `TestComponentBuilder`の廃止

RC.5で`TestBed`になりました。

### `#`シンタックスと`var-`シンタックスの廃止

非推奨でした。ローカル変数宣言には`let-`を使います

### `HTTP_PROVIDERS`と`JSONP_PROVIDERS`の廃止

それぞれ`HttpModule`と`JsonpModule`になります

### `CACHED_TEMPLATE_PROVIDER`の改名

`CACHED_TEMPLATE_PROVIDER`は`RESOURCE_CACHE_PROVIDER`に改名されました

### `NgSwitchWhen`の廃止

非推奨でした。現在は`NgSwitchCase`を使います

### `ReplacePipe`の廃止

非推奨でした。

### `UpgradeAdapter#addProvider`の廃止

非推奨でした。Angularモジュールを使います

### commonパッケージのForms APIの廃止

`@angular/common`パッケージから提供されていた古いForms APIは廃止されました。
`@angular/forms`を使います

### Web Worker関連APIのパッケージ分割

これまではplatform-browserパッケージに同梱されていましたが、今回から独立した`platform-webworker`パッケージで提供さてます。
新しく`@angular/platform-webworker`と`@angular/platform-webworker-dynamic`が公開されています。

## その他の変更

### core

#### `NO_ERRORS_SCHEMA`の追加
[feat\(core\): add NO\_ERRORS\_SCHEMA that allows any properties to be set… · angular/angular@c631cfc](https://github.com/angular/angular/commit/c631cfc)

RC.5で、テンプレート中でのカスタムエレメントの使用を許可する、`CUSTOM_ELEMENT_SCHEMA`が実装されましたが、
今回はどんなテンプレートでもエラーを発生させないスキーマが追加されました。

`CUSTOM_ELEMENT_SCHEMA`と同様に`NgModule.schema`に設定します。
基本的にアプリケーションでは使用しないAPIで、ユニットテスト時に使うことが想定されています。

```ts
TestBed.configureTestingModule({
  schemas: [NO_ERRORS_SCHEMA]
});
```

#### Animation APIのイベントオブジェクトに`totalTime`を追加
[feat\(animations\): make sure animation callback reports the totalTime … · angular/angular@4f8f8cf](https://github.com/angular/angular/commit/4f8f8cf)

Animation APIのトランジションイベントで受け取れるオブジェクトが、`AnimationTransitionEvent`というクラスのインスタンスになりました。
`AnimationTransitionEvent#totalTime`プロパティでトランジションにかかった時間を取得できます。

### compiler

#### compiler-cliのTypeScript 2.0対応
[fix\(compiler\): allow tsc\-wrapped to be compile with TypeScript 2\.0 \(\#… · angular/angular@fc2fe00](https://github.com/angular/angular/commit/fc2fe00)

今まで1.9.0-devバージョンに依存していたtsc-wrappedパッケージがTypeScript 2.0に対応しました

#### `entryComponents`から`declarations`への自動的な適用を廃止
[fix\(compiler\): do not autoinclude components declared as entry points… · angular/angular@c56f3f2](https://github.com/angular/angular/commit/c56f3f2)

これまで`entryComponents`に指定されたコンポーネントは自動的に`declarations`にも適用されていましたが、
今後はどちらも明示的に指定する必要があります。

#### テンプレート中に未知の要素が存在することを検知し、エラーを発生させるようになった
[fix\(DomSchemaRegistry\): detect invalid elements · angular/angular@1df69cb](https://github.com/angular/angular/commit/1df69cb)

HTML標準の要素でもなければAngularモジュールに登録されたディレクティブでもない未知の要素について、
テンプレート中に存在するとエラーが発生するようになりました。
その要素がWeb Components仕様のカスタムエレメントである場合は、Angularモジュールに`CUSTOM_ELEMENT_SCHEMA`を追加します。
そうでない場合は適切なディレクティブをAngularモジュールに登録します。

この件で、一部のHTML標準の要素が認められていないバグが発生しています。
たとえば `<time>`要素については、[こちらのIssue](https://github.com/angular/angular/issues/11219)を参照してください。

#### `:host`,`:host-context`の修正
[fix\(ShadowCss\): properly shim selectors after :host and :host\-context… · angular/angular@af63378](https://github.com/angular/angular/commit/af63378)

`ViewEncapsulation`がデフォルトの時、`:host`セレクターと`:host-context`セレクターがうまく適用されないバグが修正されました

### common

#### `DatePipe`がfloat形式の文字列を受け取れるように修正
[fix\(datePipe\): allow float for date pipe input \(\#10687\) · angular/angular@712c7d5](https://github.com/angular/angular/commit/712c7d5)

`"123456789.11"`のような文字列を扱えるようになりました。

### http

#### bodyが空の時に`Responce#text()`が空文字列を返すように修正
https://github.com/angular/angular/commit/7cd4741

### i18n

#### `LOCALE_ID`の導入
[feat\(i18n\): provide LOCALE\_ID and NgLocalization · angular/angular@ce4eae6](https://github.com/angular/angular/commit/ce4eae6)

i18n機能用の新たなAPIとして、アプリケーションのロケールを管理する`LOCALE_ID`が追加されました。
次のようにロケールを指定できます。デフォルトは`en-US`になっています。

```ts
providers: [{provide: LOCALE_ID, useValue: 'en-US'}]
```

#### `CurrencyPipe`,`DatePipe`,`NumberPipe`がロケールを参照するように変更
https://github.com/angular/angular/commit/0a053a4

これまでは`en-US`で固定だったこれらのパイプが、`LOCALE_ID`を参照するようになりました。

### router

#### ルーティングのカスタムエラーハンドリングをサポート
[feat\(router\): add support for custom error handlers · angular/angular@2fc5c57](https://github.com/angular/angular/commit/2fc5c57)

`RouterModule.forRoot()`の第2引数で、ルーティングエラーのハンドル用関数を渡せるようになりました。
また、`Router#errorHandler`に対しても関数をセット可能です

```ts
RouterModule.forRoot(appRoutes, {
    errorHandler: (error: any) => { 
        // ...
    }
});
```

```ts
this.router.errorHandler = (error: any) => {
    // ...
};
```

#### `RouterTestingModule.withRoutes`の追加
[feat\(router\): add syntax sugar for confuguring RouterTestingModule \(\#… · angular/angular@53c99cf](https://github.com/angular/angular/commit/53c99cf)

ユニットテスト用の`RouterTestingModule`に`withRoutes`関数が実装され、
`provideRoutes()`関数を別に呼び出す必要がなくなりました

```ts
TestBed.configureTestingModule({
    imports: [
      RouterTestingModule.withRoutes(
          [{path: '', component: BlankCmp}, {path: 'simple', component: SimpleCmp}]),
      TestModule
    ]
});
```      

----

以上、RC.6の主な変更点でした。量こそ多いですがほとんどは非推奨APIの廃止です。
計画通りなら、次は2.0.0 Finalリリースです。
今月末にはロンドンでAngularConnectも開催されますので楽しみにしていましょう。

[AngularConnect 2016 \- Europe’s Largest Angular Conference](http://angularconnect.com/)

それではまた。
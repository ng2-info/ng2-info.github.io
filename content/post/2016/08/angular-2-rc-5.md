+++
date = "2016-08-10T21:13:11+09:00"
tags = ["Release", "RC"]
title = "Angular 2 RC.5がリリースされました"

+++

どうも、らこです。ついにAngular 2のRC.5がリリースされました。
2.0最終リリースに向けた最後の大規模アップデートということで、変更の量は凄まじいものがあります。
`NgModule` APIを始めとした既存APIの刷新は、一見バージョンアップ対応のハードルは高そうに見えますが、
RC.5に合わせた書き方に直していけば、これまでよりもわかりやすく直感的なAPIになっていることが感じられるはずです。

それでは重要な変更点をピックアップしていきます。

<!--more-->

[2.0.0-RC.5](https://github.com/angular/angular/blob/master/CHANGELOG.md#200-rc5-2016-08-09)

## `NgModule`について

新しい`NgModule` APIについては、 [こちらの記事](/2016/07/preparing-for-ngmodule)で簡単な解説をしているので、まずはそちらを読んでください。

## 破壊的変更

まずは破壊的変更についてまとめておきます。

### bootstrappingに関する変更

一番大きな変更は、アプリケーションのbootstrappingに関するAPIの変更です。
`NgModule`を前提とした新しいAPIが標準となり、今までのコンポーネントベースのbootstrapingは非推奨になりました。

アプリケーションを実装するにあたって、まずはモジュールを作ることになります。モジュールは次のように`@NgModule`デコレータを使います。

```ts
import {NgModule} from '@angular/core';

@NgModule({
  declarations: […], // モジュールに読み込むディレクティブ、コンポーネント、パイプ
  imports: [BrowserModule], // 依存するモジュール
  providers: […], // DIプロバイダ
  boostrap: [MainComponent], // アプリケーションのエントリポイントになるコンポーネント
})
class MyAppModule {}
```

こうして宣言したモジュールは、次のようにbootstrappingします。

```ts
import {platformBrowserDynamic} from ‘@angular/platform-browser-dynamic’;

platformBrowserDynamic().bootstrapModule(MyAppModule);
```

AoT(Ahead of Time)コンパイルを利用する場合は、次のようになります

```ts
import {platformBrowser} from ‘@angular/platform-browser’;

platformBrowser().bootstrapModuleFactory(MyAppModuleNgFactory);
```

#### Web Worker向けAPIの変更

これまでWeb Workerでアプリケーションを起動するには`bootstrapWorkerApp`を使っていましたが、
`WorkerAppModule`と`workerAppPlatform()`を使って、モジュールをbootstrapすることになります。

`bootstrapWorkerUi`についても、`WorkerUiModule`をインポートして、`workerUiPlatform()`でbootstrapします。

#### Server向けAPIの変更

`serverBootstrap`は廃止され、アプリケーションのモジュールを`serverDynamicPlatform()`でbootstrapすることになります

### `Compiler`に関する変更

コンポーネントコンパイラは常に`Compiler`クラスでInjectするようになりました。
`RuntimeCompiler`クラスや`OfflineCompiler`クラスを直接Injectすることはできません。

### `ApplicationRef`に関する変更

アプリケーションの主体がモジュールに移行したことから、`ApplicationRef`関連のAPIの多くが非推奨になりました。

#### `ApplicationRef#waitForAsyncInitializers`の廃止

アプリケーションの初期化が完了したことを受け取るためのAPIだった`ApplicationRef#waitForAsyncInitializers`が廃止されました。
代わりに`AppInitStatus#donePromise(): Promise`か、`AppInitStatus#done: boolean`を使います。
`AppInitStatus`は新しく導入されたクラスで、Injection経由でインスタンスを取得できます。

#### `ApplicationRef.registerBootstrapListener`の廃止

bootstrapが完了した後に呼び出されるイベントリスナーを登録するためのAPIだった`ApplicationRef.registerBootstrapListener`が廃止されました。
代わりに、`APP_BOOTSTRAP_LISTENER`トークンを使って、multi providerを設定します。

```ts
[
    {
        provide: APP_BOOTSTRAP_LISTENER,
        multi: true,
        useValue: (cmp: ComponentRef) => {
            // bootstrapされたコンポーネントのComponentRefが渡される
        }
    }
]
```

#### `ApplicationRef#dispose`の廃止

アプリケーションを破棄する`ApplicationRef#dispose`が廃止されました。
代わりにモジュールを破棄する`NgModuleRef#destroy`を使います。

#### `AplicationRef#registerDisposeListener`の廃止

アプリケーションが破棄されたイベントを受け取るためのAPIが廃止されました。
代わりにモジュールのクラスで`ngOnDestroy`ライフサイクルメソッドを実装するか、
`NgModuleRef`クラスをInjectして、`NgModuleRef#onDestroy`メソッドでイベントリスナーを登録します。

#### `ApplicationRef#run`の廃止

`ApplicationRef#run`は廃止され、代わりに`NgZone#run`を直接使うようになります。

#### `ApplicationRef#injector`の廃止

`ApplicationRef#injector`は廃止され、代わりに`Injector`を直接Injectするか、`NgModuleRef#injector`を使います。

#### `ApplicationRef#zone`の廃止

`ApplicationRef#zone`は廃止され、`NgZone`を直接使います。

### `disposePlatform`の廃止

`disposePlatform`は`destroyPlatform`に名前が変わりました。
全体として`dispose`から`destroy`に統一するためです。

同様に、`PlatformRef#dipose()`から`PlatformRef#destroy()`に、
`PlatformRef#registerDisposeListener`から`PlatformRef.onDestroy`に、
`PlaformRef#diposed`から`PlatformRef#destroyed`に、それぞれ改名されました。

### テンプレートスキーマに関する挙動の変更

デフォルトでは、テンプレート中の未知の要素に対するデータバインディングはエラーを起こすようになっています。
ただし、`CUSTOM_ELEMENTS_SCHEMA`を有効にすると、要素名に`-`を含む場合はその要素をCustom Elementsとして扱い、既知の要素として扱います。

`CUSTOM_ELEMENTS_SCHEMA`は`@NgModule`の`schemas`に設定します。

```ts
@NgModule({
    declarations: [MyComponentThatUsesAWebComponent],
    imports: [BrowserModule],
    schemas: [CUSTOM_ELEMENTS_SCHEMA],
    boostrap:  [MyComponentThatUsesAWebComponent],
})
export class MyAppModule{}
```

### `coreLoadAndBootstrap`/`coreBootstrap`の廃止

`coreLoadAndBootstrap`と`coreBootstrap`は廃止されました。代わりには`bootstrapModule`か`bootstrapModuleFactory`を使います。

### RouteConfigと`declarations`に関する規定

ルート設定に含まれるすべてのコンポーネントは、モジュールの`declarations`に含まれている必要があります。
これはJITコンパイル、AoTコンパイル、遅延ロードによらず常に必要です。

### テスティングAPIに関する変更

`@angular/core/testing`をはじめとするテスティングAPIも、`NgModule`を前提としたAPIに刷新されました。

#### `TestInjector`の廃止と`TestBed`の導入

これまでユニットテスト中のInjectionを管理していた`TestInjector`が廃止され、
代わりにユニットテスト用のモジュールを管理する`TestBed` APIが導入されました。

`withProviders`は`TestBed.withModule`に、`addProviders`は`TestBed.configureTestingModule`に変更されます。
`TestComponentBuilder`も`TestBed.createComponent`に変更されます。

Example: 

```ts
import {TestBed} from '@angular/core/testing';

describe('TestComponent', () => {
  beforeEach(() => {
    // テスト用モジュールのセットアップ
    TestBed.configureTestingModule({
      declarations: [TestComponent]
    });
  });

  it('component test', async(() => {
    // Component設定の上書き
    TestBed.overrideComponent(TestComponent, {set: {
      template: '<p>Content</p>'
    }});
    // コンポーネントのコンパイル
    TestBed.compileComponents().then(() => {
      // コンポーネントインスタンスの作成 
      var fixture = TestBed.createComponent(TestComponent);
      fixture.detectChanges();
      var compiled = fixture.debugElement.nativeElement;

      expect(compiled).toContainText('Content');
    });
  }));
});
```

テスト環境のセットアップを行う`setBaseTestProviders`も、`TestBed.initTestEnvironment()`に変更されます。

Before:

```ts
import {setBaseTestProviders} from '@angular/core/testing';
import {
    TEST_BROWSER_DYNAMIC_PLATFORM_PROVIDERS,
    TEST_BROWSER_DYNAMIC_APPLICATION_PROVIDERS
} from '@angular/platform-browser-dynamic/testing';

setBaseTestProviders(
    TEST_BROWSER_DYNAMIC_PLATFORM_PROVIDERS,
    TEST_BROWSER_DYNAMIC_APPLICATION_PROVIDERS
);
```

After: 

```ts
import {TestBed} from '@angular/core/testing';
import {BrowserDynamicTestingModule, platformBrowserDynamicTesting} from '@angular/platform-browser-dynamic/testing';

TestBed.initTestEnvironment(
    BrowserDynamicTestingModule,
    platformBrowserDynamicTesting()
);
```

### `ngModel`の挙動の変更と、ユニットテストへの影響

`ngModel`は常に非同期的な挙動を取るようになりました。
つまり、ユニットテスト時には`ComponentFixture#detectChanges`の呼び出しではなく、
`ComponentFixture#whenStable`を使って非同期的に変更の反映を待つ必要があります。

Before:

```ts
let fixture = TestBed.createComponent(InputComp);
fixture.detectChanges();

let inputBox = <HTMLInputElement> fixture.debugElement.query(By.css('input')).nativeElement;
expect(inputBox.value).toEqual('Original Name');
```

After: 

```ts
let fixture = TedBed.createComponent(InputComp);
fixture.whenStable().then(() => {
    let inputBox = <HTMLInputElement> fixture.debugElement.query(By.css('input')).nativeElement;
    expect(inputBox.value).toEqual('Original Name');
});
```

### Formsのモジュール化

`@angular/forms`を使用するためのAPIが変わります。今後は各フォームAPI用のモジュールをインポートします。

Before:

```ts
import {disableDeprecatedForms, provideForms} from @angular/forms;

bootstrap(App, [
  disableDeprecatedForms(),
  provideForms()
]);
```

After: 

```ts
import {DeprecatedFormsModule, FormsModule, ReactiveFormsModule} from @angular/common;

@NgModule({
  declarations: [MyComponent],
  imports: [BrowserModule, DeprecatedFormsModule],
  boostrap:  [MyComponent],
})
export class MyAppModule{}
```

### `PLATFORM_PIPES`と`PLATFORM_DIRECTIVES`の廃止

どちらも`NgModule#declarations`を使うようになりました

### Routerの挙動の変更

クエリパラメータやフラグメントなど、URLに付随する要素は、デフォルトでは保持されなくなりました。
ナビゲーションの際に保持したい場合は、`preserveQueryParams`/`preserveFragments`オプションをtrueにします。

```ts
router.navigate(["/"], { preserveQueryParams: true });
```

routerLinkの場合は、`[preserveQueryParams]`として設定します。


```html
<a routerLink="/" [preserveQueryParams]="true">top</a>
```

### アニメーションのテンプレートシンタックスの変更

アニメーションのトリガーの式は`@prop`という書き方でしたが、`[@prop]`に変更されました。

```html
// 非推奨
<div @trigger="expression"></div>

// 新しい書き方
<div [@trigger]="expression"></div>
```

### ngUpgrade関連の変更

ngUpgradeもモジュール対応しました。

Before: 

```ts
let upgradeAdapter = new UpgradeAdapter();
upgradeAdapter.addProviders([myProvidersArray);
```

After: 

```ts
@NgModule({
  providers: myProvidersArray
})
class MyModule {}

let upgradeAdapter = new UpgradeAdapter(MyModule);
```

## その他の変更

破壊的変更以外にも要注意の変更はいくつもあるので、それぞれ見ていきます。

### core

#### ngForの挙動の修正 
[e18626b](https://github.com/angular/angular/commit/e18626b)

ngForの挙動が修正され、必要なときだけ要素の追加・移動・削除を行うようになりました。
この変更はアニメーションに影響するものです。

#### `selector`を持たないコンポーネントを許容するように修正 
[9b39e49](https://github.com/angular/angular/commit/9b39e49)

これまでは`selector`が設定されていないコンポーネントを動的に読み込んだとき、DOM上は`<undefined>`として展開されていましたが、
今後は`<ng-component>`として展開されるようになりました。

### compiler

#### ngIfの右辺でパイプが使えるように修正 
[8efbcc9](https://github.com/angular/angular/commit/8efbcc9)

`*ngIf`の右辺の式でパイプが使えるようになりました。

#### Safe Operator(`?.`)の評価の挙動を修正 
[4ec2a30](https://github.com/angular/angular/commit/4ec2a30)

`a?.b.c`という式について`a`がnullあるいはundefinedだった場合、これまでは`a`をnullと評価したあとに`b`の評価に進んでしまい、
`a?.b?.c`と書かないとエラーになっていました。
この修正により、`a`の時点でSafe Operatorが働いた場合はその先の評価を行わなずに全体を`null`とするように修正されました。

### common

#### `CurrencyPipe`の規定の挙動の変更 
[d455942](https://github.com/angular/angular/commit/d455942)

`CurrencyPipe`の各パラメータの規定値が、Intl APIのデフォルトを使うようになりました。

#### `NumberPipe`がstring型を許容するようになった 
[f3dd91e](https://github.com/angular/angular/commit/f3dd91e)

`NumberPipe`が文字列型の数値も許容するようになりました。

### http

#### HTTPヘッダの処理をcase-insensitiveに修正 
[7f64782](https://github.com/angular/angular/commit/7f64782)

これまでケースセンシティブになっていたのが修正されました

#### `content-type`ヘッダの上書きをサポート 
[bdb5912](https://github.com/angular/angular/commit/bdb5912)

`body`に渡されたオブジェクトの型から自動的に判別される`content-type`は、
`headers`に明示的に指定されたもので上書きできるようになりました。

#### `arraybuffer`型と`blob`型のレスポンスをサポート 
[1266460](https://github.com/angular/angular/commit/1266460), [76b8a49](https://github.com/angular/angular/commit/76b8a49)

`Response#arrayBuffer()`と`Response#blob()`がサポートされました。

#### OPTIONSメソッドをサポート 
[0bd97ec](https://github.com/angular/angular/commit/0bd97ec)

HTTPのOPTIONSメソッドのための`Http#options()`が追加されました。

### router

routerについては個別のチェンジログがあります

[Router CHANGELOG](https://github.com/angular/angular/blob/master/modules/@angular/router/CHANGELOG.md)

#### `RouterModule`の導入

Routerもモジュール化され、`RouterModule`をインポートするようになります。

```ts
@NgModule({
    imports: [RouterModule.forRoot(appRoutes, {enableTracing: true})]
})
class MyAppModule {}
```

#### クエリパラメータの値が空の時の挙動を修正 
[0d6cc17](https://github.com/angular/angular/commit/0d6cc17)

値が空(`?key=`)のクエリパラメータを含むURLで初回のルーティングを行うと、値に`true`がセットされてしまうバグを修正しました。

#### `canActivateChild`/`canLoad`の追加

ガードの種類に、すべての子ルートのアクティベーションをチェックする`canActivateChild`と、
遅延ロードが可能かどうかをチェックする`canLoad`が追加されました

#### `RouterOutlet`のイベントを追加

`<router-outlet>`要素から`activate`イベントと`deactivate`イベントが発火されるようになりました。

#### GuardとResolverがPromiseをサポート

各種GaurdとResolverのインターフェースで、戻り値としてPromiseが使えるようになりました。
Observableと同様に、Promiseを返した場合は完了するまでナビゲーションが待機されます。

#### `ActivateRoute`が`routeConfig`を返せるようになった

現在アクティブなルートの設定を`ActivateRoute`から取得できます。

#### `queryParams`や`fragment`が`ActivateRoute`から取得するようになった

これまで`Router#routerState`から取得していたクエリパラメータやフラグメントは`ActivateRoute`から取得するようになりました

### forms

#### `NgForm#reset`の追加 
[da8eb9f](https://github.com/angular/angular/commit/da8eb9f)

フォームを初期化する`NgForm#reset()`が追加されました

#### multipleなselect要素で`change`イベントを使うように修正 
[3cbded6](https://github.com/angular/angular/commit/3cbded6)

これまで`input`イベントで処理していたバグが修正されました。

#### フォーム内のコントロールを簡単に取得するためのAPIを追加 
[8d44999](https://github.com/angular/angular/commit/8d44999)

フォーム内のコントロールを名前で取得できる`get()`メソッドが追加されました。

```ts
@Component({
  selector: 'my-app',
  template: `
    <div>
      <form #f="ngForm">
        <input name="a" ngModel>
      </form>
      <button (click)="checkForm(f)">Check Form</button>
    </div>
  `,
})
export class App {
  checkForm(f: NgForm) {
    f.form.get("a") // FormControl
  }
}
```

#### コントロールに`invalid`/`pending`プロパティを追加 
[e0eea6c](https://github.com/angular/angular/commit/e0eea6c)

コントロールの状態を取得するプロパティが追加されました。

### animation

#### `Component#host`プロパティでのアニメーショントリガーの設定をサポート 
[806a254](https://github.com/angular/angular/commit/806a254)

`@Component`の`host`プロパティでアニメーションのトリガー設定ができるようになりました。
この結果として、Routerによるナビゲーションにアニメーションを付与することができるようになります。
`[@routeAnimation]`をtrueに設定しておくと、`routeAnimation`トリガーで、表示(`void => *`)と、離脱(`* => void`)のアニメーションを記述できます。

```ts
@Component({
  selector: 'home',
  template: `Home`,
   host: {
     '[@routeAnimation]': 'true',
   },
  animations: [
    trigger('routeAnimation', [
      state('*', style({transform: 'translateX(0)', opacity: 1})),
      transition('void => *', [
        style({transform: 'translateX(-100%)', opacity: 0}),
        animate(1000)
      ]),
      transition('* => void', animate(1000, style({transform: 'translateX(100%)', opacity: 0})))
    ])
  ]
})
class HomeCmp {}
```

実際に動くサンプルは[こちら](http://plnkr.co/edit/hH667v6ODDc28vBqCBvT?p=preview)です

<iframe src="http://embed.plnkr.co/hH667v6ODDc28vBqCBvT/?show=preview" frameborder="0" width="100%" height="300"></iframe>  

----

RC.5の変更点のまとめは以上です。かなり抜粋しましたがそれでもこの量です。
今回のアップデートではAoTコンパイルに関する情報も出てきていますが、この記事では扱いません。
後日AoTコンパイルの手順については改めて書こうと思います。

それではFinalリリースまであと少し、頑張っていきましょう。よい夏を！

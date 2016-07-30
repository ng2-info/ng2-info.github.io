+++
title= "Beta.16とBeta.17の変更点"
date= "2016-04-29T23:36:29+09:00"
tags = ["Beta","Release","BreakingChange"]

+++

どうも、らこです。
今週はBeta.16とBeta.17の2つのリリースがありまして、特にBeta.16はベータ始まって以来の最大級の変更がリリースされているので、
みなさんにはぜひとも頑張ってBeta.17へのアップデートを乗り越えて欲しいところです。
破壊的変更は多いですが、基本的なAPIについては機械的に修正可能なものがほとんどです。
逆に、Angular 2の深いところまで潜っていた方ほど被害が大きいでしょう。
それでは重要な変更をピックアップしていきます。

<!--more-->

---

## [CHANGELOG](https://github.com/angular/angular/blob/master/CHANGELOG.md)

### `Location` が `angular2/platform/common` に移動しました
これまで `angular2/router` モジュールから提供されていた `Location` クラスが、
新しく生まれた `angular2/platform/common` モジュールに移動しました。
つまり Core側のパッケージに含まれることになり、 `angular2/router` に依存せずに使えるようになります。

また、 `Location` に関連する `APP_BASE_HREF` や `LocationStrategy` などのAPIも移動しています。
以前は次のようにインポートしていましたが、

```ts
import {
  PlatformLocation,
  Location,
  LocationStrategy,
  HashLocationStrategy,
  PathLocationStrategy,
  APP_BASE_HREF}
from 'angular2/router';
```

今後は次のようにインポートするようになります。

```ts
import {
  PlatformLocation,
  Location,
  LocationStrategy,
  HashLocationStrategy,
  PathLocationStrategy,
  APP_BASE_HREF}
from 'angular2/platform/common';
```

### `Injector` が読み込み専用になり、 `ReflectiveInjector` が追加されました
Offline Compileの実装に伴い、`Injector`に大きな変更が入りました。

[Injector - ts](https://angular.io/docs/ts/latest/api/core/Injector-class.html)

まず第一に、 `Injector` クラスが抽象クラスとなり、 `get` メソッドだけを提供するようになりました。
これまで `Injector`が提供していた他のメソッドは、具象クラスである `ReflectiveInjector` が実装しています。

[ReflectiveInjector - ts](https://angular.io/docs/ts/latest/api/core/ReflectiveInjector-class.html)

```ts
var injector = ReflectiveInjector.resolveAndCreate([]);
expect(injector.get(Injector)).toBe(injector);
```

また、`getOptional` メソッドが廃止され、 `get` メソッドが第2引数としてデフォルト値を取るようになりました。
デフォルト値を設定しない時にプロバイダが見つからなかった時は今までどおり例外が発生します。

```ts
injector.get(optionalDependency, notFoundValue);
```

### `Compiler`の廃止と`ComponentFactory`の導入
コンポーネントを動的にコンパイルするためのAPIとして、これまでは`Compiler`が提供されていましたが、
これが廃止され、新たに`ComponentResolver`と`ComponentFactory`という2つのAPIが追加されました。

[refactor(core): introduce ComponentFactory. · angular/angular@0c600cf](https://github.com/angular/angular/commit/0c600cf6e31b7f69f5a36f7dea959e4884217a4d)

`ComponentResolver`は基本的に従来の`Compiler`とほとんど変わらないAPIを持っています。

```ts
// beta.15
export abstract class Compiler {
  abstract compileInHost(componentType: Type): Promise<HostViewFactoryRef>;
  abstract clearCache();
}

// beta.17
export abstract class ComponentResolver {
  abstract resolveComponent(componentType: Type): Promise<ComponentFactory>;
  abstract clearCache();
}
```

`ComponentResolver`はプロバイダを記述しなくてもコンポーネントやディレクティブでインジェクト可能です。

`ComponentResolver#resolveComponent`が返す`ComponentFactory`は
後述する`ViewContainerRef.createComponent`のメソッドの引数として使うことができます。
また、`ComponentFactory.create` メソッドを使えば、ビューへの挿入なしに、`ComponentRef`だけを生成できます。

### `ViewContainerRef.createHostView` が `.createComponent`に改名されました
よりわかりやすい名前に変わり、戻り値の型も `HostViewRef` から `ComponentRef` に変わりました。

さらに、`ResolvedProvider`クラスが廃止された影響で、第3引数は`Injector`を要求するようになりました。
もしInjectorを渡したい場合は、専用の新しい子Injectorを作ってあげるのが推奨されます。

```ts
let childInjector = ReflectiveInjector.resolveAndCreate(…);
vcRef.createComponent(cmpFactory, index, childInjector)
```

### `DynamicComponentLoader.loadIntoLocation` が廃止されました
[refactor(core): add `Query.read` and remove `DynamicComponentLoader.l… · angular/angular@efbd446](https://github.com/angular/angular/commit/efbd446d18e6e0380beafcad6e94a7751d788623)

指定した要素をコンテナとして動的にコンポーネントをビューに追加するAPIだった `DynamicComponentLoader.loadIntoLocation` が廃止されました。
代わりに、指定した要素の次の位置に追加する `DynamicComponentLoader.loadNextToLocation` が引数として`ElementRef`ではなく`ViewContainerRef`を要求するようになりました。
つまり、動的にコンポーネントをビューへ追加するには必ず`ViewContainerRef`が必要になったということです。

`ViewContainerRef`はコンポーネントやディレクティブが自身のものをDIで取得することができます。
また、コンポーネントのテンプレート中から任意の要素の`ViewContainerRef`を得るには、`@ViewChild`を使います。
通常、`@ViewChild("loc")`とすると`#loc`が付与された要素の`ElementRef`が得られますが、
第2引数として `{read: ViewContainerRef}`とすることでコンテナを取得することができます

```ts
@Component({
    selector: 'my-comp',
    template: '<div #loc></div>'
})
class MyComp {
  ctxBoolProp: boolean;

  @ViewChild('loc', {read: ViewContainerRef}) viewContainerRef: ViewContainerRef;

  constructor(private loader: DynamicComponentLoader){}

  loadChildComponent() {
    this.loader.loadNextToLocation(OtherComponent, this.viewContainerRef)
        .then(componentRef => {
            ...
        });
  }
}
```

### `AppViewManager` が廃止されました
低レイヤーのビュー管理APIだった `AppViewManager`が内部専用のAPIになり、外部には公開されなくなりました。
`AppViewManager`でできることは`DynamicComponentLoader`と`ViewContainerRef`で同じことができます。

### `ComponentRef.dispose` が `ComponentRef.destroy`に改名されました
名前が変わっただけです。ライフサイクルの`ngOnDestroy`に合わせてわかりやすくした形です。

### 非同期テストのやりかたが大きく変わりました
`angular2/testing`での非同期テストが大きく変わりました。

まず最初に、非同期テストを行うために `zone.js/dist/async-test`の読み込みが必要になりました。

```ts
import "zone.js/dist/async-test";
```

また、APIも変わっています。これまではDIを行いつつ非同期テストを行うには`injectAsync`関数を使っていましたが、
DIを行う`inject`関数と、非同期テストを行う`async`関数の2つに分離されました


```ts
// Before:

it('should wait for returned promises', injectAsync([FancyService], (service) => {
  return service.getAsyncValue().then((value) => { expect(value).toEqual('async value'); });
}));
it('should wait for returned promises', injectAsync([], () => {
  return somePromise.then(() => { expect(true).toEqual(true); });
}));

// After
it('should wait for returned promises', async(inject([FancyService], (service) => {
  service.getAsyncValue().then((value) => { expect(value).toEqual('async value'); });
})));
it('should wait for returned promises', async(() => {
  somePromise.then() => { expect(true).toEqual(true); });
}));
```

`async`関数に渡された関数は、そのテスト固有のZoneが生成され、
そのZoneが非同期処理の完了を監視してくれるので、`done`のような明示的なテスト終了処理は不要です。
Promiseをreturnする必要もなくなりました。

ちなみに、`fakeAsync`も同様です。
`fakeAsync`を使う際には追加で `zone.js/dist/fake-async-test`の読み込みが必要になり、
使い方も`fakeAsync(inject([...], (...) => {...}))`のように変わります。

### `Renderer.renderComponent`が廃止されました
任意のコンポーネントをレンダリングする低レベルAPIだった`Renderer.renderComponent`が廃止されました。
同じAPIは`RootRenderer.renderComponent`として提供されています。

### ビューのクエリの仕様が変わりました
[refactor(view_compiler): codegen DI and Queries · angular/angular@2b34c88](https://github.com/angular/angular/commit/2b34c88)

`@ViewQuery`や`@ViewChild`、`@ContentChild`などのビュークエリは、`DynamicComponentLoader`によって動的に読み込まれたコンポーネントには適用されない、という仕様に変わりました。
例えば、`<router-outlet>`によって読み込まれるコンポーネントは、クエリ対象になりません。
ただし、`<router-outlet>`は`activate`イベントを発火するので、
新しく読み込まれたコンポーネントの`ComponentRef`を取得することができます。
動的にコンポーネントを読み込む場合は同じようにイベントを発火してあげるようにしましょう。

### Change Detectionの処理順序が変わりました
Change Detectionの処理順序が次のようになります。

1. Inputのチェック
    1. `ngOnChanges`
    2. `ngOnInit` (一度のみ)
    3. `ngDoCheck`
2. Contentの更新
    1. ContentのChange Detection
    2. `ContentChildren`の更新
    3. `ngAfterContentChecked`
3. Viewの更新
    1. ViewのChange Detection
    2. `ViewChildren`/`ViewQuery`の更新
    3. `ngAfterViewChecked`

### Pipeのパラメータの仕様が変更されました
これまで、Pipeの`transform`メソッドには第2引数の型が`args: any[]`だったので常に配列が渡されていましたが、
`...args: any[]`に変わり、直接オブジェクトを受け取れるようになりました。

```ts
// Before
@Pipe({name: "repeat"})
class RepeatPipe implemetes PipeTransform {
    transform(value: any, args: any[]): any {
        let times = <number>args[0]; // 常に配列なので0番目を取得する必要があった
        return value.repeat(times);
    }
}

// After
@Pipe({name: "repeat"})
class RepeatPipe implemetes PipeTransform {
    transform(value: any, times: number): any {
        return value.repeat(times);
    }
}
```

### `#...`シンタックスの仕様変更と`let-`/`ref-`シンタックスの追加
[feat(core): separate refs from vars. · angular/angular@d2efac1](https://github.com/angular/angular/commit/d2efac1)

これまで、`ngFor`の中で使われる`#...`は反復中のオブジェクトを示し、それ以外では付与された要素の参照を示していましたが、
これは混乱を招いていました。

そこで、テンプレート内でのローカル変数を作るためのシンタックスとして新しく`let-`が追加されました。

```html
<!--Before-->
<li *ngFor="#item of items; #i = index">...</li>
<template ngFor="#item of items; #i = index"></div>
<template ngFor #item [ngForOf]="items" #i="index"><li>...</li></template>

<!--After-->
<li *ngFor="let item of items; let i = index">...</li>
<template ngFor="let item of items; let i = index"></div>
<template ngFor let-item [ngForOf]="items" let-i="index"><li>...</li></template>
```

また、`#...`シンタックスはこれまで`var-...`と対応していましたが、今後は`ref-...`になります。
通常の要素に付与すればその要素の参照に、`<template>`要素に付与すれば`TemplateRef`として扱えます。

`var-`シンタックスは将来的に廃止される非推奨なAPIとなりました。
`let-`か`ref-`のどちらかに書き直しましょう。

### `TemplateRef`にコンテキストのジェネリクスが付きました
先述の`let-`と関連して、ローカル変数をオブジェクトとして扱うためのコンテキストが導入されました。
`TemplateRef`は、自身のコンテキストの型をジェネリクスとして宣言する必要があります

例えば、`NgFor`は`NgForRow`というコンテキストを持っています。

```ts
export class NgForRow {
  constructor(public $implicit: any, public index: number, public count: number) {}

  get first(): boolean { return this.index === 0; }

  get last(): boolean { return this.index === this.count - 1; }

  get even(): boolean { return this.index % 2 === 0; }

  get odd(): boolean { return !this.even; }
}

...

export class NgFor implements DoCheck {
  ...

  constructor(private _viewContainer: ViewContainerRef, private _templateRef: TemplateRef<NgForRow>,
              private _iterableDiffers: IterableDiffers, private _cdr: ChangeDetectorRef) {}

  ...
}
```

`ViewContainerRef.createEmbeddedView`を使って`TemplateRef`からビューを作るときに、第2引数としてコンテキストオブジェクトを渡すことができます。

```ts
class SomeViewportContext {
  constructor(public someTmpl: string) {}
}

@Directive({selector: '[someViewport]'})
@Injectable()
class SomeViewport {
constructor(container: ViewContainerRef, templateRef: TemplateRef<SomeViewportContext>) {
  container.createEmbeddedView(templateRef, new SomeViewportContext('hello'));
  container.createEmbeddedView(templateRef, new SomeViewportContext('again'));
  }
}

<template someViewport let-greeting="someTmpl">
    <p>{{greeting}}</p>
</template>
```

コンテキストを使ってローカル変数を設定できるようになったので、
従来の`EmbeddedViewRef.setLocal`は削除されました。

### `NgTemplateOutlet`の追加
`TemplateRef`を渡すと内部のViewContainerにセットしてくれる便利なディレクティブが追加されました。

```html
<div>
  <template #tmp>
    <h1>Template!!</h1>
  </template>
  <div [ngTemplateOutlet]="tmp"></div>
</div>
```

簡単に`router-outlet`のようなビューの切り替えが実装できるようになります。

---

**長い！！！！！！**

お疲れ様でした。Beta.16, 17ではOffline Compileのために基盤部分が大きく変わっており、
深いAPIを使っているほど影響が大きいアップデートです。
冒頭にも言ったように、このアップデートに対応しておかないと今後の追従が難しいので、
被害の大きかった人も頑張って対応しましょう。

Offline Compileの使い方はまだドキュメントがなく、CLIがまだ開発途上らしいのでもうしばらく時間がかかりそうです。

それでは。








+++
date = "2016-07-30T20:53:25+09:00"
tags = ["RC"]
title = "NgModule導入について"

+++

どうも、らこです。RC.5のリリースがおそらく来週と迫っていますが、多くのバグ修正と共に新しい機能が追加されます。
**NgModule**はこれまでのAngular2で不便だったこと、複雑だったことを一気に解決してくれる新機能です。

RCも大詰めとなったこのタイミングで導入されることに困惑するかもしれませんが、
ぜひとも対応してもらいたいと思います。

<!--more-->

## はじめに

NgModuleは完全に新しく導入されたAPIであり、既存のAPIへの破壊的変更ではありません。
ただし、従来の方法は非推奨となり、stableリリースの段階では廃止される予定です。
RC.5からは移行期間に入るものと思っていてください。

## NgModule

NgModuleの概要についてスライドを作ってあるので、これをベースに解説します。

<iframe src="//slides.com/laco/ng2-ngmodule-overview/embed" width="100%" height="420" scrolling="no" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

### NgModuleの概要

NgModuleは、ディレクティブやパイプ、サービスなどをひとまとめにしたモジュールを宣言するためのAPIです。
`@Component`などと同じようにデコレータを使って宣言します。

アプリケーションの起動に最低限必要なモジュールは次のようになります。

```ts
import {NgModule, ApplicationRef} from '@angular/core';
import {BrowserModule} from '@angular/platform-browser';
import {AppComponent} from './app.component';

@NgModule({
    declarations: [AppComponent],
    imports: [BrowserModule],
    bootstrap: [AppComponent]
})
class AppModule {
}
```

そして、NgModuleで宣言したモジュールを使ってアプリケーションを起動するための、新しいbootstrap関数があります。

```ts
import {AppModule} from './app.module';
import {platformBrowserDynamic} from '@angular/platform-browser-dynamic';

platformBrowserDynamic().bootstrapModule(AppModule);
```

NgModuleで作ったモジュールを、各プラットフォームの`bootstrapModule`メソッドで起動するという流れになります。

### NgModuleの中身

それではNgModuleデコレータについてもう少し詳しく見ていきましょう

#### declarations

このプロパティは、そのモジュールの中で宣言されている**ディレクティブ**と**パイプ**を登録する場所です。
このディレクティブにはもちろんコンポーネントも含みます。

```ts
@NgModule({
     declarations: [
        AppComponent,
        MyComponent,
        MyDirective,
        MyPipe,
        ...SOME_LIBRALIES_DIRECTIVES
    ],
})
class AppModule {}
```

これまでは、`FooDirective`を宣言したあと、それを使うには`@Component`の`directives`プロパティに毎回追加していました。
パイプにおいても同様に`pipes`プロパティに追加する必要がありました。

NgModuleの`declarations`に登録されたディレクティブやパイプは、そのモジュール内でならどこでも使えるようになります。
つまり、自作したディレクティブ・コンポーネント・パイプはすべて`declarations`に登録しておけばよいです。

`ROUTER_DIRECTIVES`のようなライブラリからインポートしたものも当然`declarations`に追加することはできますが、
これに関しては後で説明する方法によって、そもそも`ROUTER_DIRECTIVES`が不要になります。

#### providers

このプロパティは、そのモジュールのトップレベルのプロバイダを宣言する場所です。
従来の`bootstrap`関数の第2引数に渡していた配列がそのまま移ってきたと思ってください。

```ts
@NgModule({
    providers: [
        MyService,
        SomeLibraryService,
    ],
})
class AppModule {}
```

完全に旧APIの上位互換となる `declarations` と違い、`@Component`の`providers`は非推奨にはなりません。
階層的DIの性質上、任意のコンポーネントでプロバイダを登録できる必要があるからです。
とはいえ、それはライブラリ作者のような複雑な使い方をする人のためのもので、
アプリケーションを普通に作っている中では基本的にすべてNgModuleの`providers`に登録しておけばよいです。

#### imports

NgModuleの中で一番重要と言えるのがこの`imports`です。
`imports`プロパティを使うことで、自分のモジュールに別のモジュールを取り込むことができます。

例えば、`@angular/common`や`@angular/platform-browser`から提供されている機能(ディレクティブなど)を取り込むには次のようにします。

```ts
import {NgModule} from '@angular/core';
import {BrowserModule} from '@angular/platform-browser';
import {CommonModule} from '@angular/common';

@NgModule({
    imports: [BrowserModule, CommonModule]
})
class AppModule {}
```

`imports`プロパティに登録されたモジュールからは、後述の`exports`プロパティで指定されたものと、`providers`プロパティのプロバイダが取り込まれます。

`@angular/router`も専用のモジュールを提供していますが、静的メソッドを使った少し特殊な方法を使います。
インポートと同時にルーティングの設定を渡すことで、動的に生成されたモジュールを取り込んでいます。

```ts
import {NgModule} from '@angular/core';
import {RouterModule} from '@angular/router';

import {APP_ROUTES} from './app.routes';

@NgModule({
    imports: [
        RouterModule.forRoot(APP_ROUTES, {useHash: true})
    ]
})
class AppModule {}
```

RouterModuleをインポートすれば、自動的に`ROUTER_DIRECTIVES`が取り込まれるので、どこでも`<router-outlet>`や`routerLink`を使えるようになります

#### exports

`imports`と対をなすのが、この`exports`です。
`exports`プロパティには、そのモジュールが`imports`に設定された時に提供するディレクティブとパイプを指定します。
アプリケーション中ではあまり使うことはなく、基本的にはライブラリ作者用のAPIです。

```ts
@NgModule({
    declarations: [AwesomeComponent, AwesomePipe]
    exports: [AwesomeComponent, AwesomePipe],
    providers: [AwesomeService]
})
export class AwesomeModule {
}
```

#### bootstrap

`bootstrap`プロパティには、アプリケーションのエントリポイントになるコンポーネントを指定します。
いままで`bootstrap`関数に渡していたコンポーネントを指定しておけば大丈夫です。


#### entryComponents

一番難しいのがこの`entryComponents`プロパティです。
このプロパティではオフラインコンパイルを行う時にエントリポイントとなるコンポーネントを指定します。
詳しく説明しようとするとまずオフラインコンパイルの説明からしないといけないので省略しますが、
ここに指定するコンポーネントは、

- アプリケーションのエントリポイントになるコンポーネント
- Lazy Loadingのよって実行時にあとから読み込まれるコンポーネント

以上のどちらかに当てはまるコンポーネントです。
ただし、`bootstrap`プロパティに指定してあるコンポーネントは自動的に`entryComponents`にも追加されるので、
後者の、遅延ロードされるコンポーネントだけを指定することになります。

#### schemas

スライドでは省略しましたが、`schemas`も面白い機能です。
このプロパティには、アプリケーション中で有効にするスキーマを設定できます。

現在使用可能なスキーマは`CUSTOM_ELEMENTS_SCHEMA`だけです。
このスキーマを有効にすると、Web標準のCustom ElementsをAngular 2のテンプレート中で使えるようになります。
具体的には、そのモジュール内のディレクティブとコンポーネントには一致せず、要素名に`-`を含む場合はそれをCustom Elementsとして解釈します。
Custom Elementsであると解釈すれば、その要素は標準のHTML要素と同様にデータバインディングやイベントハンドリングが許可されます。

```ts
@Component({
  selector: 'some-component',
  template: `
    <some-custom-element [someUnknownProp]="true"></some-custom-element>
  `,
})
export class SomeComponent {
}

@NgModule({
    schemas: [CUSTOM_ELEMENTS_SCHEMA], 
    declarations: [SomeComponent]
})
export class ModuleUsingCustomElements {
}
```

### RC.4からの移行

NgModuleに関して、移行する必要があるのは次のものです。

- `bootstrap`関数の呼び出し
- `@Component`の`directives`
- `@Component`の`pipes`

まずはアプリケーションのモジュールをひとつ宣言しましょう。
一般的に必要なモジュールも読み込みます。

```ts
import {NgModule} from '@angular/core';
import {BrowserModule} from '@angular/platform-browser';
import {CommonModule} from '@angular/common';
import {FormsModule} from '@angular/forms';
import {HttpModule} from '@angular/http';
import {RouterModule} from '@angular/router';

import {APP_ROUTES} from './app.routes';

@NgModule({
    imports: [BrowserModule, CommonModule, FormsModule, HttpModule, RouterModule.forRoot(APP_ROUTES)]
})
export class AppModule {}
```

次に、`bootstrap`関数に渡していたコンポーネント(`AppComponent`とする)をモジュールに追加します。

```ts
import {NgModule} from '@angular/core';
import {BrowserModule} from '@angular/platform-browser';
import {CommonModule} from '@angular/common';
import {FormsModule} from '@angular/forms';
import {HttpModule} from '@angular/http';

import {AppComponent} from './app.component';

@NgModule({
    imports: [BrowserModule, CommonModule, FormsModule, HttpModule],
    declarations: [AppComponent],
    entryComponents: [AppComponent]
})
export class AppModule {}
```

RC.5の時点では、`@Component`の`directives`や`pipes`に指定されているディレクティブやパイプは、
そのコンポーネントが`declarations`に含まれていれば自動的に巻き上げられるようになっています。
なので、とりあえず最初は`AppComponent`だけを`declarations`に追加しておけば今までどおりの動作が維持できます。

モジュールが作成できたら、bootstrapする準備をします。
まずは**モジュールからコンポーネントを起動する**処理をモジュールのコンストラクタに書きます。

```ts
import {NgModule, ApplicationRef} from '@angular/core';
import {BrowserModule} from '@angular/platform-browser';
import {CommonModule} from '@angular/common';
import {FormsModule} from '@angular/forms';
import {HttpModule} from '@angular/http';

import {AppComponent} from './app.component';

@NgModule({
    imports: [BrowserModule, CommonModule, FormsModule, HttpModule],
    declarations: [AppComponent],
    entryComponents: [AppComponent]
})
export class AppModule {

    constructor(appRef: ApplicationRef) {
        appRef.bootstrap(AppComponent);
    }
}
```

次に、**プラットフォームからモジュールを起動する**処理を書きます。
これは元々`bootstrap`関数が呼び出されていた場所を置き換えます。

```ts
import {platformBrowserDynamic} from '@angular/browser-platform-dynamic';

import {AppModule} from './app.module';

platformBrowserDynamic().bootstrapModule(AppModule);
```

これで`bootstrap`から`bootstrapModule`への移行が完了です。
ここから先は、各コンポーネントに書かれた`directives`や`pipes`を`declarations`に少しずつ回収していけばよいです

#### テストのNgModule化

現在、テスト用のAPIもモジュール対応が進められています。
こちらは残念ながら破壊的変更が行われます。

- `withProviders`から`TestBed.withModule`に変更
- `addProviders`から`TestBed.configureTestingModule`に変更
- `TestComponentBuilder`が廃止され、`TestBed.createComponent`に変更

どの変更も、DIを直接使うのではなくテスト用のモジュールを設定してテストするいう流れに変わるものです。

詳細については、公式のチェンジログが待てないというかたは該当のコミットを読むなどするとよいでしょう

- [refactor\(testing\): introduce new testing api to support ng modules · angular/angular@d0a95e3](https://github.com/angular/angular/commit/d0a95e35af943258c360486ccb2f60d11a36e97d)

## まとめ

NgModuleへの置き換えはそれほど大変ではないことがわかったと思います。
そして、今まで複雑だった部分を単純にしてくれる機能であることもわかったでしょうか。
RC.5のリリースは数日中に来ると思われるので、モジュールの波に乗り遅れないように頑張ってください。
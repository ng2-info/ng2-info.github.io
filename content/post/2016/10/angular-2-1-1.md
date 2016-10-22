+++
date = "2016-10-22T14:09:43+09:00"
title = "Angular 2.1.1/2.2.0-beta.0のリリース"
tags = ["release"]

+++

<!--more-->

どうも、らこです。今週も予定通り新しいバージョンのリリースが行われました。現在リリースされているバージョンは以下のとおりです

|    | latest | next |
|----|-------|----|
|core| 2.1.1 | 2.2.0-beta.0 |
|router | 3.1.1 | 3.2.0-beta.0|

それでは今回の注目すべき変更点をまとめます。

----

## [2.1.1](https://github.com/angular/angular/blob/master/CHANGELOG.md#211-2016-10-20)

### Bug Fixes

* **compiler:** generate aot code for animation trigger output events ([#12291](https://github.com/angular/angular/issues/12291)) ([6e5f8b5](https://github.com/angular/angular/commit/6e5f8b5)), closes [#11707](https://github.com/angular/angular/issues/11707)

アニメーションのトランジションイベントのハンドリングを含むテンプレートがAoTコンパイルできなかったバグが修正されました

* **router:** correctly export filter operator in es5 ([#12286](https://github.com/angular/angular/issues/12286)) ([27d7677](https://github.com/angular/angular/commit/27d7677))

router.umd.jsにRxJSのfilterオペレーターが含まれておらず、routerパッケージ単体で読み込んだときにエラーになっているのが修正されました

* **router:** do not update primary route if only secondary outlet is given ([#11797](https://github.com/angular/angular/issues/11797)) ([da5fc69](https://github.com/angular/angular/commit/da5fc69))

名前付きrouter-outletだけを更新する遷移のときに、デフォルトのrouter-outletを更新しないように修正されました

* **router:** fix lazy loading triggered by redirects from wildcard routes ([5ae6915](https://github.com/angular/angular/commit/5ae6915)), closes [#12183](https://github.com/angular/angular/issues/12183)

ワイルドカードルート `**` からリダイレクトした先のルートがLazy Loadingを使っていると正しくルーティングされないバグが修正されました

## [2.2.0-beta.0](https://github.com/angular/angular/blob/master/CHANGELOG.md#220-beta0-2016-10-20)

### Features

* **common:** support narrow forms for month and weekdays in DatePipe ([#12297](https://github.com/angular/angular/issues/12297)) ([f77ab6a](https://github.com/angular/angular/commit/f77ab6a)), closes [#12294](https://github.com/angular/angular/issues/12294)

`date`パイプのフォーマットに新しく1文字形式(**Narrow form**)での月と曜日の表現が追加されました

| Component | Symbol | Narrow | Short Form   | Long Form         | Numeric   | 2-digit   |
|-----------|:------:|--------|--------------|-------------------|-----------|-----------|
| month     |   M    | L (S)  | MMM (Sep)    | MMMM (September)  | M (9)     | MM (09)   |
| weekday   |   E    | E (S)  | EEE (Sun)    | EEEE (Sunday)     | -         | -         |

* **forms:** add hasError and getError to AbstractControlDirective ([#11985](https://github.com/angular/angular/issues/11985)) ([592f40a](https://github.com/angular/angular/commit/592f40a)), closes [#7255](https://github.com/angular/angular/issues/7255)

フォームのコントロールに `hasError` メソッドと `getError` メソッドが追加されました
```html
<label>Username</label><input name="username" ngModel required #username="ngModel">
<div *ngIf="username.dirty && username.hasError('required')">Username is required</div>
```

* **forms:** add ng-pending CSS class during async validation ([#11243](https://github.com/angular/angular/issues/11243)) ([97bc971](https://github.com/angular/angular/commit/97bc971)), closes [#10336](https://github.com/angular/angular/issues/10336)

非同期バリデーターによるバリデーション中に`ng-pending`クラスがセットされます

* **forms:** Added emitEvent to AbstractControl methods ([#11949](https://github.com/angular/angular/issues/11949)) ([b9fc090](https://github.com/angular/angular/commit/b9fc090))

フォームのコントロールが持つメソッドに、 `emitEvent` オプションが追加されます。
`emitEvent`がfalseのとき、そのメソッドによる操作は`valueChange`イベントや`statusChange`イベントを発火しません。
```ts
control.setValue('foo', {emitEvent: false});
```

* **forms:** make 'parent' a public property of 'AbstractControl' ([#11855](https://github.com/angular/angular/issues/11855)) ([445e592](https://github.com/angular/angular/commit/445e592))

フォームのコントロールに`parent`プロパティが追加され、入れ子になったコントロールから親のコントロールグループを取得できるようになります

* **forms:** Validator.pattern accepts a RegExp ([#12323](https://github.com/angular/angular/issues/12323)) ([bf60418](https://github.com/angular/angular/commit/bf60418))

`Validator.pattern`バリデーターがRegexp型のパターンを受け付けるようになります

* **upgrade:** add support for AoT compiled upgrade applications ([d6791ff](https://github.com/angular/angular/commit/d6791ff)), closes [#12239](https://github.com/angular/angular/issues/12239)

`@angular/upgrade`パッケージを使ったアプリケーションがAoTコンパイル可能になります。

* **router:** add support for ng1/ng2 migration ([#12160](https://github.com/angular/angular/issues/12160)) ([8b9ab44](https://github.com/angular/angular/commit/8b9ab44))

`@anguar/upgrade`を使ってAngular 1と2を共存させるアプリケーションにおいて、部分的にAngular 2のrouterを使用できるようにする機能が追加されます。
この新しい機能については、Victor Savkin氏が自身のブログで自ら記事を書いています。
[Migrating Angular 1 Applications to Angular 2 in 5 Simple Steps](https://vsavkin.com/migrating-angular-1-applications-to-angular-2-in-5-simple-steps-40621800a25b#.tucitwyc7)
Angular 2のrouterを有効にする条件を、`UrlHandlingStrategy`という仕組みで制御できるようになります。

----

2.2系に含まれるフォーム関連の機能はとても便利そうです。Angular 1からのマイグレーション関連の変更も特徴的です。とはいえまだbeta.0ですので、緊急でなければ3週間後の2.2.0リリースを待ちましょう。

それではまた来週のリリースで。

+++
title = "Angular 2.1.2/2.2.0-beta.1のリリース"
tags = [
    "release"
]
date = "2016-10-28T13:53:56+09:00"

+++

<!--more-->

どうも、らこです。今週も予定通り新しいバージョンのリリースが行われました。現在リリースされているバージョンは以下のとおりです

|    | latest | next |
|----|-------|----|
|core| 2.1.2 | 2.2.0-beta.1 |
|router | 3.1.2 | 3.2.0-beta.1 |

それでは今回の注目すべき変更点をまとめます。

---

## [2.1.2](https://github.com/angular/angular/blob/master/CHANGELOG.md#212-2016-10-27) 

### Bug Fixes

#### **compiler:** walk third party modules ([#12453](https://github.com/angular/angular/issues/12453)) ([a838aba](https://github.com/angular/angular/commit/a838aba)), closes [#11889](https://github.com/angular/angular/issues/11889) [#12428](https://github.com/angular/angular/issues/12428)

サードパーティライブラリが提供するDirectiveを使ったときに、AoTコンパイルで生成されるコードが正しくないimport文を含むバグが修正されました

#### **compiler:** use Maps instead of objects in selector implementation ([d321b0e](https://github.com/angular/angular/commit/d321b0e))

ComponentやDirectiveのselectorとクラスのマッピングに使っていたオブジェクトを、Mapに置き換えました。
今までは単なるObjectのインスタンスだったので、一部の文字列(`constructor`など)は特別な意味をもってしまい、selectorとして使えませんでした。

#### **compiler-cli:** assert that all pipes and directives are declared by a module  ([7221632](https://github.com/angular/angular/commit/7221632))

ngcコンパイラが、NgModuleのdeclarationsに含まれていないPipeやDirectiveが見つかったときにも警告を出すようになりました。

#### **http:** overwrite already set xsrf header ([b4265e0](https://github.com/angular/angular/commit/b4265e0))

リクエストの際に明示的に`X-XSRF-TOKEN`ヘッダを設定してある場合はhttpパッケージによって自動的に付与されているものを上書きするように修正されました

#### **router:** canDeactivate guards are not triggered for componentless routes ([b741853](https://github.com/angular/angular/commit/b741853)), closes [#12375](https://github.com/angular/angular/issues/12375)
#### **router:** change router not to deactivate aux routes when navigating from a componentless routes ([52a853e](https://github.com/angular/angular/commit/52a853e))

Componentを持たないルートについて、deactivateに関するバグが修正されました。

#### **router:** disallow component routes with named outlets ([8f2fa0f](https://github.com/angular/angular/commit/8f2fa0f)), closes [#11208](https://github.com/angular/angular/issues/11208) [#11082](https://github.com/angular/angular/issues/11082)

Componentを持たないルートがRouterOutletの名前を指定したときにエラーを出すようになりました。

#### **router:** preserve resolve data ([6ccbfd4](https://github.com/angular/angular/commit/6ccbfd4)), closes [#12306](https://github.com/angular/angular/issues/12306)

一度resolveされた値を、`ActivatedRoute`が保存するようになりました。

### その他

ngcによって生成されるngfactoryファイルのサイズを小さくする最適化が行われています。

## [2.2.0-beta.1](https://github.com/angular/angular/blob/master/CHANGELOG.md#220-beta1-2016-10-27)

### Code Refactoring

#### **upgrade:** re-export the new static upgrade APIs on new entry ([a26dd28](https://github.com/angular/angular/commit/a26dd28))

upgradeパッケージのAPIをimportするパスが変更されました。

  - downgradeComponent
  - downgradeInjectable
  - UpgradeComponent
  - UpgradeModule

以上のAPIは `@angular/upgrade/static` からexportされるようになります。

### Features

#### **router:** export routerLinkActive w/ isActive property ([c9f58cf](https://github.com/angular/angular/commit/c9f58cf))

`RouterLinkActive` ディレクティブが `routerLinkActive` としてテンプレート中でexportされます。
次のように`routerLinkActive`ディレクティブをテンプレート変数として取り出し、`isActive`プロパティにアクセスできます。

```html
<a routerLink="/user/bob" routerLinkActive #rla="routerLinkActive">
    Bob {{ rla.isActive ? '(already open)' : ''}}
</a>
```

----

今週は追加でいくつか。

### 記事紹介

- [Handling Service Configuration Without A Configuration Phase In Angular 2\.1\.1](https://www.bennadel.com/blog/3174-handling-service-configuration-without-a-configuration-phase-in-angular-2-1-1.htm)

Angular 2.1系における、Serviceのデザインパターンです。`multi: true`なproviderを使って柔軟な設定を行うサービスを作る方法を紹介しています。
Dependency Injectionの有効な活用法なので参考にすると良いでしょう

- [Reactive FormGroup validation with AbstractControl in Angular 2](https://toddmotto.com/reactive-formgroup-validation-angular-2)

最近リニューアルしたTodd Motto氏のブログの新しい記事です。ReactiveFormsのFormGroupに対してカスタムバリデーターを作成、適用する方法を解説しています

### その他

- Angular-CLIの**1.0.0-beta.19-3**がリリースされました
- Universalプロジェクトのコアであるplatform-nodeモジュールがAngularの本体に吸収されることになりました
- 上記と関連して、Core TeamのメンバーによるUniversalへのコントリビューションが増えるとのことです

----

今週も順調にリリースが進んでいます。また、Universalの開発をCore Teamが加速させるのはとても期待できますね。完成が楽しみです

それではまた来週のリリースで。

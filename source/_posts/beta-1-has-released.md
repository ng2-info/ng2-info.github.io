title: Angular2 Beta.1のリリース
date: 2016-01-11 14:03:31
tags: 
- Beta
- Release
---

Angular2のBeta.1バージョンがリリースされました。

[CHANGELOG](https://github.com/angular/angular/blob/master/CHANGELOG.md)

---
# 2.0.0-beta.1 catamorphic-involution (2016-01-08)


### Bug Fixes

* **benchpress:** fix flake ([9d28147](https://github.com/angular/angular/commit/9d28147)), closes [#6161](https://github.com/angular/angular/issues/6161)
* **CHANGELOG:** typo ([d116861](https://github.com/angular/angular/commit/d116861)), closes [#6075](https://github.com/angular/angular/issues/6075) [#6078](https://github.com/angular/angular/issues/6078)
* **code size:** revert previous devMode change to restore size targets ([c47d85b](https://github.com/angular/angular/commit/c47d85b))
* **core:** IE only supports parentNode ([630d931](https://github.com/angular/angular/commit/630d931)), closes [#5994](https://github.com/angular/angular/issues/5994)
* **docs:** fix an import in TOOLS_DART.md ([3524946](https://github.com/angular/angular/commit/3524946)), closes [#5923](https://github.com/angular/angular/issues/5923)
* **forms:** fix SelectControlValueAccessor not to call onChange twice ([b44d36c](https://github.com/angular/angular/commit/b44d36c)), closes [#5969](https://github.com/angular/angular/issues/5969)
* **router:** correctly sort route matches with children by specificity ([b2bc50d](https://github.com/angular/angular/commit/b2bc50d)), closes [#5848](https://github.com/angular/angular/issues/5848) [#6011](https://github.com/angular/angular/issues/6011)
* **router:** preserve specificity for redirects ([a038bb9](https://github.com/angular/angular/commit/a038bb9)), closes [#5933](https://github.com/angular/angular/issues/5933)
* **TemplateParser:** do not match on attrs that are bindings ([9a70f1a](https://github.com/angular/angular/commit/9a70f1a)), closes [#5914](https://github.com/angular/angular/issues/5914)

### Features

* **core:** improve NoAnnotationError message ([197cf09](https://github.com/angular/angular/commit/197cf09)), closes [#4866](https://github.com/angular/angular/issues/4866) [#5927](https://github.com/angular/angular/issues/5927)
* **core:** improve stringify for dart to handle closures ([e67ebb7](https://github.com/angular/angular/commit/e67ebb7))
* **core:** speed up view creation via code gen for view factories. ([7ae23ad](https://github.com/angular/angular/commit/7ae23ad)), closes [#5993](https://github.com/angular/angular/issues/5993)
* **router:** support links with just auxiliary routes ([2a2f9a9](https://github.com/angular/angular/commit/2a2f9a9)), closes [#5930](https://github.com/angular/angular/issues/5930)

### Performance Improvements

* **dart/transform:** Avoid unnecessary reads for files with no view ([89f32f8](https://github.com/angular/angular/commit/89f32f8)), closes [#6183](https://github.com/angular/angular/issues/6183)


### BREAKING CHANGES

* Platform pipes can only contain types and arrays of types,
  but no bindings any more.
* When using transformers, platform pipes need to be specified explicitly
  in the pubspec.yaml via the new config option
  `platform_pipes`.
* `Compiler.compileInHost` now returns a `HostViewFactoryRef`
* Component view is not yet created when component constructor is called.
  -> use `onInit` lifecycle callback to access the view of a component
* `ViewRef#setLocal` has been moved to new type `EmbeddedViewRef`
* `internalView` is gone, use `EmbeddedViewRef.rootNodes` to access
  the root nodes of an embedded view
* `renderer.setElementProperty`, `..setElementStyle`, `..setElementAttribute` now
  take a native element instead of an ElementRef
* `Renderer` interface now operates on plain native nodes,
  instead of `RenderElementRef`s or `RenderViewRef`s
  
---

いくつかのバグフィックスの他に、エラーメッセージの改善やRouterの機能追加がありますが、メインはView生成機構の破壊的変更ですね。
この件に関しては[過去の記事](/2016/01/06/view-creation-changes/)で取り上げています。

RouterはAuxRouteについてもrouterLinkで`<a [routerLink]="['/', ['Modal']]">open modal</a>`のようにリンクを生成できるようになりました。[#5930](https://github.com/angular/angular/pull/5930)
 
 
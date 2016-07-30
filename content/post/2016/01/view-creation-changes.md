+++
title = "Viewの生成に関する破壊的な変更"
date = "2016-01-06T12:56:45+09:00"
tags = ["BreakingChange"]
+++

あけましておめでとうございます。新年一発目のBreakingChangesのお知らせです。

<!--more-->

[refactor(core): speed up view creation via codegen for view factories.](https://github.com/angular/angular/pull/5993)

毎度のことながら`refactor`がリファクタリングじゃないですね。変更点は以下のとおり。

---
BREAKING CHANGE:
- Platform pipes can only contain types and arrays of types,
  but no bindings any more.
- When using transformers, platform pipes need to be specified explicitly
  in the pubspec.yaml via the new config option
  `platform_pipes`.
- `Compiler.compileInHost` now returns a `HostViewFactoryRef`
- Component view is not yet created when component constructor is called.
  -> use `onInit` lifecycle callback to access the view of a component
- `ViewRef#setLocal` has been moved to new type `EmbeddedViewRef`
- `internalView` is gone, use `EmbeddedViewRef.rootNodes` to access
  the root nodes of an embedded view
- `renderer.setElementProperty`, `..setElementStyle`, `..setElementAttribute` now
  take a native element instead of an ElementRef
- `Renderer` interface now operates on plain native nodes,
  instead of `RenderElementRef`s or `RenderViewRef`s

目的はRendererのさらなる高速化で、一番重要なのは「ComponentはコンストラクタでViewを生成しない」という点です。CompilerやRendererのAPIに関しても大きな変更がありますが、普通のアプリケーションを作る上ではあまり触らない部分なので被害者はそれほど多くないのではないでしょうか。たとえ使っていてもおそらくビルドが通らなくなるのでバージョンアップ後にすぐに気づいて修正できるでしょう。

今回の変更に関して、リリース前に対策すべきなのはComponentのコンストラクタで何かしらの処理を行っているケースです。Viewが生成前なのでコンストラクタでの処理がViewの初期化によって上書きされるおそれがあります。コンストラクタは極力DIの解決だけにとどめ、初期化処理は`ngOnInit()`を使用しましょう。

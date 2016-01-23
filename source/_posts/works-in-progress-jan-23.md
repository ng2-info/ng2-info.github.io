title: 1/23 最近のAngular2の動き
date: 2016-01-23 21:27:40
author: laco0416
tags:
- Planning

---

Beta.1以降しばらく目立った動きがないですが、Beta.2に向けていくつかの面白い変更があるので紹介します。

### DDCビルドの必須化
Dartの新コンパイラであるDart Dev Compiler(DDC)は現在も開発中ですが、
Angular2のCI設定が変わり、DDCでビルドが通ることが必須になりました。

[dart-lang/dev_compiler: Experimental Dart to JavaScript compiler designed to create idiomatic, readable JavaScript output.](https://github.com/dart-lang/dev_compiler)

[chore(ddc): make DDC build non-experimental by yjbanov · Pull Request #6647 · angular/angular](https://github.com/angular/angular/pull/6647)

DDCもちょうど1年くらい開発されていますし、そろそろ安心して使えるコンパイラになってきたんでしょうか。
TypeScript/JavaScript側には何も影響ないですが、Dartのソースコードの変更はDDCでもビルドできるように気を使わないといけません。

### プロジェクトのnpm v3化
開発環境のNodeバージョンが5.4系に上がり、それに合わせてnpmもv3系になりました。
依存しているパッケージも一気にアップデートされています。

[build: npm dependencies update + dedupe by IgorMinar · Pull Request #6213 · angular/angular](https://github.com/angular/angular/pull/6213)

今まではgit cloneしたあとのnpm installがnpm v2じゃないと行えなかったので、
npm v3になることでコントリビュートしやすくなりました。

しかしAngular2のnpm配布パッケージは未だpeerDependenciesを使っており、RxJSやZone.jsなどを別にインストールしないといけない状況は変わっていません。

### Angular2の型定義ファイルに関する変更
現在、Angular2とjQueryの型定義が共存できない問題があります。

[Type definition conflict with jQuery TSD · Issue #5459 · angular/angular](https://github.com/angular/angular/issues/5459)

グローバルの`$`変数が衝突しており、手作業でどちらかの型定義ファイルを編集しなければ解決できません。
直接の原因はAngular2の配布パッケージに含まれる型定義が、E2Eテスト用のパッケージである`angular-protractor`の型定義を含んでいたことです。
テストにしか使わないもので本来必要ない型定義なので、通常のビルドとテスト用のビルドで依存する型定義を切り替える変更が入りました。

[build(node): split test and src compilation units by vicb · Pull Request #6262 · angular/angular](https://github.com/angular/angular/pull/6262)

これがリリースされれば、AngularJSが依存しているjQueryとの衝突もなくなるはずなので、1系からのマイグレーションもやりやすくなるはずです。

---

おそらく来週にはBeta.2がリリースされる雰囲気（あくまでも雰囲気）なので、アップデートを楽しみにしましょう。

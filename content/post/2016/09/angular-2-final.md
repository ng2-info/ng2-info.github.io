+++
date = "2016-09-16T00:36:17+09:00"
tags = ["Release"]
title = "🎉✨ Angular 2.0.0がリリースされました 🎉✨"

+++

どうも、らこです。

ついに**Angular 2.0.0が正式リリースされました**！めでたい！！
今回はRC.7も含めた変更点の解説に加えて、正式リリース後のAngularの予定についても紹介します。

<!--more-->

## RC.6からの変更点

RC.7、2.0.0ともに、ほとんどがバグ修正とドキュメンテーションの追加で、破壊的な変更はRC.7にひとつあるだけです。

[2.0.0-RC.7](https://github.com/angular/angular/blob/master/CHANGELOG.md#200-rc7-2016-09-13)
[2.0.0](https://github.com/angular/angular/blob/master/CHANGELOG.md#200-2016-09-14)

### 破壊的変更

**`...Metadata`系のクラスを廃止**
[refactor\(core\): remove \`…Metadata\` for all decorators and use the dec… · angular/angular@63e15ff](https://github.com/angular/angular/commit/63e15ff)

RC.6までは、`@Component`デコレーターに渡すオブジェクトの型は`ComponentMetadata`というクラスになっていて、
`Component`以外にもそれぞれのデコレーターに対応した`...Metadata`クラスが存在しました。

RC.7で`...Metadata`クラスはそのデコレーターと同じ名前のインターフェースに統一されました。

* Before: `new ComponentMetadata(…)`
* After: `new Component(…)`

### 依存パッケージの更新

2.0.0時点で、依存パッケージはそれぞれ次のバージョンに更新されました

* zone.js@0.6.21
* rxjs@5.0.0-beta.12

### 修正されたバグ(抜粋)

**ShadowCSSのパフォーマンスリグレッションを修正**
[fix\(ShadowCss\): fix perf regression \(\#11420\) · angular/angular@78ad9ad](https://github.com/angular/angular/commit/78ad9ad)

RC.6リリースによって起きていた、ShadowCSSの重大なパフォーマンス問題を修正しています。

**実装漏れのHTML5標準要素をスキーマに追加**
[fix\(DomSchema\): add missing elements · angular/angular@d309f77](https://github.com/angular/angular/commit/d309f77)

`<time>`要素や`<summary>`要素など、HTML5仕様に含まれているがAngularが認識できていなかった要素をスキーマに取り入れました。

**`Title`サービスが`BrowserModule`から提供されていなかったのを修正**
[fix\(platform\-browser\): provide Title service as part of the module \(\#… · angular/angular@85d9db6](https://github.com/angular/angular/commit/85d9db6)

`platform-browser`から提供されている、ウィンドウのタイトルを操作するための`Title`サービスが、`BrowserModule`に含まれていなかったのを修正しています。

----

## Angularの今後

さて、2.0.0正式リリースを迎えたAngularの、今後の動きについて紹介していきます。
正式リリースが発表されたイベントでの発言からいくつかポイントを抜粋しています。

[Angular Special Event \- YouTube](https://www.youtube.com/watch?v=xTIWBXkpvDc)

### Semantic Versioningの導入

Angular 2.0.0から、[Semantic Versioning](http://semver.org/)に従ってバージョンを付与するようになりました。
また、**最低6ヶ月間**は破壊的変更を含めず、メジャーバージョンを維持することを宣言しています。
つまり、最短で2017年の2月ごろに、Angularはバージョン3.0.0を迎えるということです。

これまでと全く違うバージョニングに戸惑う方も多いと思いますが、
Angular 1から2のような断絶を避けるために、今後は後方互換性を保ったまま緩やかにアップデートを続けていきます。
Angular 2の正式版に至るまでと同じように、将来的に廃止される予定のAPIは非推奨なAPIにして、
段階的にマイグレーションが可能になるようなアップデートを予定しています。

新しいバージョニングに合わせて、AngularJSやAngular2といった呼称はすべて、**Angular**に統一されることになります。
現在のバージョン2のことは、Angular 2.0や、Angular v2という風に呼ぶことになります。

### Angular 2の今後

Angular 2としてこれから取り組む項目は次のとおりです。

* 多くのバグ修正と、破壊的でない機能追加
* ドキュメンテーションの拡充
* アニメーションAPIの拡充
* Angular Material 2の拡充
* Web Worker向けAPIの安定化
* Angular Universalの拡充と、対応言語の拡張
* 高速化と軽量化

今後数ヶ月間、Angularチームはこれらを中心に取り組んでいきます。

### Angular 1のサポートについて

Angular 1のサポートは、今後も長い間続けられます。
ひとつの指標として、Angular 2がAngular 1よりもメジャーになるまでは、Angular 1をしっかりサポートすると言っています。
現在Angular 1のユーザーは約130万人、それに対してAngular 2のユーザーは48万人程度です。
これが逆転するまでは、ひとまずAngular 1はこれまでどおりメンテナンスされ続けるでしょう。

そして、シェアの逆転が起きたところで、それはすぐにAngular 1が終わりになるわけではなく、
その後もしばらくはコアチームによってアップデートが続けられる予定です。

### AoTについて

今後Angularは、デフォルトでAoTを用いるようにする予定です。
現在はcompiler-cliを使ってコンパイルしていますが、近いうちにWebpackだけで解決できるような仕組みを提供する予定があると話しています。

### AngularCLIについて

AngularCLIはついにデフォルトでWebpackを使うようになり、使いやすさが格段にあがりました。
今後はコアチームのメンバーも積極的にコントリビュートして、AngularCLIをより便利にしていく予定です。
スキャフォールディングのカスタマイズ機能についても前向きに検討しているとのことです。

----

以上、RC.6からの変更点と、Angular 2.0の今後についてのまとめでした。
いきなりではありましたが、何はともあれ嬉しいニュースです。
いままで正式リリースじゃないという理由で迷っていた方も、ぜひ秋の夜長にAngular 2に入門してみてください。

それではまた。
+++
title= "今週のニュース"
date ="2016-04-21T00:06:01+09:00"
tags =["Meeting","Beta","BreakingChange"]

+++

どうも、らこです。
今週は忙しくてミーティングノートの記事が出せませんでしたが、ざっくりと今週の知っておきたいトピックだけを紹介します。

<!--more-->

----

## `NgTemplateOutlet`ディレクティブが追加されます

次回リリースのBeta.16にて、`NgTemplateOutlet`ディレクティブが追加されます。
このディレクティブは、`TemplateRef`を渡すことで、別の要素にビューを埋め込むことができます。
`ngIf`や`ngFor`のような仕組みを、もっと簡単に活用できるようになります。

```ts
@Directive({
  selector: [myDir],
  template: '<template [ngTemplateOutlet]="currentTplRef"></template>',
  directives: [NgTemplateOutlet]
})
class MyDir {

  currentTplRef: TemplateRef;

  constructor(private currentTplRef: TemplateRef) {}
}
```

```html
<div *myDir>
  This is going to be inserted into myDir.
</div>

<!-- or alternatively -->

<template myDir>
  <div>This is going to be inserted into myDir.</div>
</template>
```

- [feat(NgTemplateOutlet): add NgTemplateOutlet directive by pkozlowski-opensource · Pull Request #8021 · angular/angular](https://github.com/angular/angular/pull/8021)

## `NgSwitchWhen`が`NgSwitchCase`に変更されます
近いうちに、`NgSwitchWhen`がリネームされ、`NgSwitchCase`になります。
AngularJS時代の名残でしたが、JavaScriptのswitch-case文と一貫性を持とうというモチベーションでリネームされることになりました。
当然ながら古い`When`のコードは動かなくなる破壊的変更なので、心づもりをしておきましょう。

- [Rename `ngSwitchWhen` to `ngSwitchCase` · Issue #7571 · angular/angular](https://github.com/angular/angular/issues/7571)

## Offline Compileがいよいよ本格的に始動します
`@Component`や`@Inject`などを事前にコンパイルしておき、起動後の初期化処理を超高速にしようという狙いのOffline Compileがいよいよmasterに投入されます。
簡単にいえば実行時のリフレクションを行わないようにすることが可能になり、
その分実行時に必要なライブラリコードも減ります。
ツリーシェイキングの余地が広がり、Rob WormaldはRollup.jsを使ったビルドでライブラリ本体が45KBを下回ったと報告しています。

Offline Compileは数段階のリリースが行われ、第2段はまた先のバージョンで導入されます。

- [Angular Weekly Meeting - Google ドキュメント](https://docs.google.com/document/d/150lerb1LmNLuau_a_EznPV1I1UHMTbEl61t4hZ7ZpS0/edit#heading=h.dzhz5dcmfn1g)

----

また、いくつか面白い記事も投稿されているので紹介します

## [Angular 2 + React Native](http://angularjs.blogspot.jp/2016/04/angular-2-react-native.html?m=1)
Angular公式ブログのゲストエントリー第1弾です。Angular 2でReact Nativeのアプリケーションを作る話です

## [Angular 2 + Meteor: the Javascript stack of the future](http://angularjs.blogspot.jp/2016/04/please-welcome-our-friend-uri.html)
こちらもAngular公式ブログのゲストエントリーです。Angular 2のMeteorのインテグレーションの話です

## [Angular 2 authentication with Auth0 and NodeJS @toddmotto](https://toddmotto.com/angular-2-authentication)
我らがTodd Motto先生によるAuth0を使ってAngular 2のアプリケーションで認証を行う話です

## [Angular 2 Universal: Set up a SEO friendly website - devcross.net](http://blog.devcross.net/2016/04/17/angular-2-universal-seo-friendly-website/)
SEOに重きをおいたAngular Universalの記事です

----

また、先日開催したng-sake #2の発表資料も紹介します。

[ng-sake #2 - connpass](http://ng-sake.connpass.com/event/29591/)


### Angular 2とMVVM (@laco0416)

<script async class="speakerdeck-embed" data-id="bc5fe15adeea4ce2bb35606f4694f183" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

私の発表資料です。2時間くらいで殴り書いたので雑ですが、MVVMはAngularで目指すものじゃないというのを言いたかっただけです。

### Angular2 + Electronでプレゼンテーションツールを作っている (@joe_re)

<script async class="speakerdeck-embed" data-id="fc65518f927d4c33b230e110f7b399f9" data-ratio="1.41241379310345" src="//speakerdeck.com/assets/embed.js"></script>

まさに紹介されているツールでプレゼンしていてかっこよかったです。こういう具体的な活用の発表をもっと聞きたいですね。

### SVG Performance with Angular (@Quramy)

<script async class="speakerdeck-embed" data-id="ebf71e74bdf7481daf3b16f4545d285c" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

SVGの描画という観点でAngular 1とAngular 2、どのくらいパフォーマンスに違いがあるのかという面白い発表です



ng-sake 第2回は驚きの低欠席率(25人中3人)で、部屋が少し窮屈で申し訳なかったです。
来月もやりますのでぜひご参加ください。

[ng-sake #3 - connpass](http://ng-sake.connpass.com/event/30746/)

----

次のBeta.16はBetaに入ってから一番と言っていいくらい大きなリリースなので、備えましょう。
それでは。


参考リンク

- [5thingsAngular - Issue #2](http://5thingsangular.github.io/2016/04/18/issue-2.html)

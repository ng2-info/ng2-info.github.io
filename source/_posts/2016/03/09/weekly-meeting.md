title: 週次ミーティング、RCリリースに関する情報他
date: 2016-03-09 21:45:29
author: laco0416
tags:
- Meeting
- RC
- AngularJS
---

どうも、らこです。今週も週次ミーティングの内容をチェックしていきます。ですがその前にまずは重大なニュースを紹介します。

## Google Preps Angular 2 for Final Release
http://thenewstack.io/google-preps-angular-2-final-release/

先週投稿された記事です。GoogleがAngular 2の **Final Release** の準備を始めているというニュースを取り上げています。
初めは半信半疑だったのですが、この記事に続いて先日GitHubのリポジトリ上でRelease Candidateのマイルストーンが作成され、
さらにBradが次のようにツイートしていることから、確実な情報だと思われます。

<blockquote class="twitter-tweet" data-lang="ja"><p lang="en" dir="ltr">We&#39;re in the chute for Angular 2 final. Follow along at home via our milestone: <a href="https://t.co/2U9urkadAd">https://t.co/2U9urkadAd</a> <a href="https://t.co/7Nv6zfOq27">pic.twitter.com/7Nv6zfOq27</a></p>&mdash; Brad Green (@bradlygreen) <a href="https://twitter.com/bradlygreen/status/707291878777982976">2016, 3月 8</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

つまり、これらの文脈での **Final** とはRCリリースのことを意味しているらしく、完全なStableリリースではないようです。
記事の情報を信じるならば、ng-confあたりでRCの開始となってもおかしくありません。実に楽しみですね。


というわけでここからは週次ミーティングのまとめです。

## Angular Weekly Meeting
### Finalリリースに向けた準備
先述のRCリリースに向けた仕上げの調整を始めているようです。まずは今できている機能の確認や、リポジトリの整理などを行います。
さらにドッグフーディングをもっと行って品質の向上を図っていくようです。

### Angular 1.5.1のリリースを急ぐ
AngularはGitHubと連携しているTravisだけではなく、**google3** という名前の社内CIサーバーでもビルドのチェックを行っています。
このCIサーバーが詰まっているらしくAngular 1.5.1がいつまでたってもリリースできない状態になっています。
IgorとMatiasがこれを解決するための作業を行うので、解決でき次第1.5.1のリリースがされるはずです。

### ブログ投稿予定
RCリリースに向けてCore TeamメンバーやGDEが続々とブログ記事を出していきます。

- AlexはOffline Compile TemplateのHelloWorldについて書く予定でしたが、この機能自体がRC段階では見送りになったのでおそらく書かれません。
- RobはAngular 2を使ったProgressive Web Appsについて書く予定です。
- Jeffはモバイル対応に関する全体的な話を書く予定です
- MatiasはテンプレートやCSSなどのパーサについて書く予定です
- Hansはangular-cliについて
- Karaは初心者向けの入門記事と、trackByに関する記事
- VictorはAngular 2のテンプレートによる利点について

公開され次第ng2-infoでも紹介していくつもりです。

## Angular 1.x Meeting
### 複数のjQueryのバージョンでテストするように
jQuery v3がリリースされるので、Angular 1.xのCIでもちゃんとテストするように対応するようです。

### 1.5.xで壊れていたBatarangの修正リリース
次期リリースで1.5.x対応できるようです

### Component RouterをAngularJS側に組み込む計画
現在はngcomponentrouterパッケージとしてリリースされているAngular 1用Component Routerを、AngularJSのリポジトリに組み込む計画です。
実現すればngRouteはおそらく廃止されるでしょう。

### HashPrefixのデフォルトを`!`に変える計画
地味にでかい破壊的変更の計画があります。hashbangでのルーティングのデフォルトURLが
`mydomain.com/#/a/b/c`から`mydomain/#!/a/b/c.`に変わります。元の挙動に戻すには`$locationProvider`でhashPrefixを変更します。

```
appModule.config(['$locationProvider', function($locationProvider) {
  $locationProvider.hashPrefix("");
}]);
```

唐突な変更で戸惑うかもしれませんが、発端はこのIssueです。

[$location.hash inserts two hash signs · Issue #13812 · angular/angular.js](https://github.com/angular/angular.js/issues/13812)

`$location.hash("foo")`でURLに`#foo`を追加しようとすると、ルートURLにいた時に`##foo`になってしまう問題を解決するための変更です。
この変更が入れば、`index.html#!#foo`という風に`##`を回避できます。

おそらく1.5.2か1.6に入ってくる変更でしょう。PushStateを使っていれば影響がないので切り替えておくのも手かもしれません。

----

今週はBeta.9が出るか出ないか怪しいところです。RCリリースに向けてすごく忙しそうなのでしばらくはBeta.8かもしれません。ゆったり待ちましょう。
それでは。

- [Google Preps Angular 2 for Final Release - The New Stack](http://thenewstack.io/google-preps-angular-2-final-release/)
- [Angular Weekly Meeting - Google ドキュメント](https://docs.google.com/document/d/150lerb1LmNLuau_a_EznPV1I1UHMTbEl61t4hZ7ZpS0/edit#heading=h.wyupmi438jru)
- [Angular 1.x Meeting Notes - Google ドキュメント](https://docs.google.com/document/d/1xKEbydyUEOQ_gTbcbxy_k2myctG8EiVbeMgLgXTxIc0/edit#)


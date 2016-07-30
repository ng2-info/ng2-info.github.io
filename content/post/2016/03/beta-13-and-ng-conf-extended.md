+++
title = "Beta.13リリースとng-conf extendedのお知らせ"
date = "2016-03-31T22:14:33+09:00"
tags = ["Beta","Release","Event"]
+++

どうも、らこです。今週もAngular 2のアップデートが行われ、Beta.13になりました。主な変更点をかいつまんで紹介します。

<!--more-->

# Beta.13リリース

## デコレータメタデータの静的解決が実装されました
将来的に実装されるOffline Template Compileのための基礎となる機能が実装されました。
コンポーネントやディレクティブなどのデコレータで定義している設定(メタデータ)にJavaScriptを実行することなくアクセスできるようにする仕組みです。
これまではコンポーネントのテンプレートは実行時に文字列が評価されるので静的に解析するのが難しかったですが、
この仕組みによって予めメタデータの情報だけを抽出できるようになり、ツールによる静的解析に大きく寄与します。

さっそくcodelyzer(ng2lintから名前が変わりました)で使う計画もあるようです。

- [feat(build): Persisting decorator metadata · angular/angular@ae876d1](https://github.com/angular/angular/commit/ae876d1)
- [Use metadata reader · Issue #11 · mgechev/codelyzer](https://github.com/mgechev/codelyzer/issues/11)

## `*ngIf`などの`*-`系ディレクティブをng-contentで扱いやすくなりました
これはバグ修正に含めるべきな気もしますが、今まで`*-`系のディレクティブが付いた要素はng-contentのselectで選択するときに`template`要素としてしかselectできませんでしたが、
この変更によって直接選択できるようになります。

次の要素がcontentになっていたとして、

```
<p *ngIf="condition" foo></p>
```

以前はtemplateでしかselectできませんでしたが

```
    // Use the implicit template for projection
    <ng-content select="template"></ng-content>
```

これからは直接selectできるようになります

```
    // Use the actual element for projection
    <ng-content select="p[foo]"></ng-content>
```

- [feat(Compiler): Allow overriding the projection selector · angular/angular@aa966f5](https://github.com/angular/angular/commit/aa966f5)

## i18n関連機能の実装が終わりました

<blockquote class="twitter-tweet" data-cards="hidden" data-lang="ja"><p lang="en" dir="ltr">Angular 2 beta 13 is out! The i18n support begins in this release...<a href="https://t.co/nsyUVw2Rpr">https://t.co/nsyUVw2Rpr</a> <a href="https://t.co/QZU4ZfyR6s">pic.twitter.com/QZU4ZfyR6s</a></p>&mdash; Angular (@angularjs) <a href="https://twitter.com/angularjs/status/715390643732762625">2016年3月31日</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

i18nを実現するためのコンポーネント、ディレクティブ、サービスなどがようやく出揃い、i18n実装完了ということになったようです。
ただしi18n実装のトラッキングイシューでは全然完了してないので、公式ドキュメントが出るまでは待つほうがよさそうです。
急ぎで国際化を行いたい場合は、[ocombe/ng2-translate](https://github.com/ocombe/ng2-translate)を使うと良いでしょう。

- [Tracking: i18n support · Issue #7480 · angular/angular](https://github.com/angular/angular/issues/7480) 

----

## ng-conf extendedのお知らせ

ソルトレイクシティで5/4から開催されるAngular最大のイベントng-confのライブビューイングイベントを開催します。

[ng-conf extended Tokyo - connpass](http://connpass.com/event/29136/)

上のイベントページはすべて英語で書いていますが、ng-confも当然英語のセッションですのでご了承ください。
日本に住んでいる外国人の方も何人か来てくれそうなので、英語で話ができるいい機会にもなるんじゃないでしょうか。

ちなみにイベントページにも書いていますが、ソルトレイクシティは日本と時差が15時間あるので、イベントの開始が日本時間の夜中0時で、終了が朝9時です。
結構ヘビーなので体力に自信のない方は自宅でゆったり見てもらったほうがよいと思います。

----

Angular 2のフットプリントのサイズを小さくする件について少しずつ動きが出ています。
まずは現在配布中のmin.jsが軒並み壊れている問題を解決するようです。
その次に、npmでインストールしたライブラリを含めてアプリケーションをminifyするときに壊れないようにするための手段を用意してくれるようです。
2,3週間以内に動きがあると思うので楽しみに待ちましょう。

それでは。
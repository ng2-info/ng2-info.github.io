+++
title = "週次ミーティングとBeta.10予告"
date = "2016-03-15T23:59:19+09:00"
tags = ["Meeting" ,"Beta" ,"BreakingChange" ,"RC" ,"ComponentRouter"]
+++

こんばんは、らこです。今週も週次ミーティングの内容をかいつまんで紹介していきます。

<!--more-->

## 3/14 週次ミーティング

### Batarangleの更新
Angular 2用のBatarangleがBeta.8に対応したようです。というよりもBeta.8で追加されたAPIを元に解析するので、
Beta.8以前のAngular 2アプリケーションはBatarangleでデバッグできなくなりました。

新機能としてRouterやDIのInjector周りのビジュアライズができるようになったらしいです。
今後はComponentやRoute、DIなどをリアルタイムでオンオフ切り替えしながらビューを比較できるようなことを目指すらしいです。期待。

### Angular 2 Final進捗報告
Angular 2 Finalに向けて大きなカテゴリごとに進捗報告です。

#### Code Generation
今まであまり表に出ていなかった計画ですが、Code Generation(コード生成)による爆速化に向けて作業が進められています。

Code Generationとは、現在bootstrap時に計算しているリフレクションを事前に計算しておき、起動処理時間を短縮する試みです。
具体的には`@Component`や`@Directive`などのメタデータの計算結果をコードとして生成します。作業は[このPR](https://github.com/angular/angular/issues/6270)で行われています。
現在はまだTypeScript/JavaScript版でしか動作せず、Dart版でも使えるようにしている段階ですが、完成すればざっくり5倍は速くなるそうです。

![Imgur](http://i.imgur.com/FU7jlvH.png)

#### ng2-material
今週Alpha版のリリースがあるようです。最初はAlpha.0からスタートで、`ngButton`、`ngCard`など基本的なパーツだけを含んでいます。
今後のアップデートで要素はもっと増えていく予定です。

#### Gesture
モバイルのタッチイベントなどに対応するライブラリも開発中で、ng2-materialからも利用される予定です。

#### 進捗具合
現在のところ20%程度です。

- https://github.com/angular/angular/milestones

<blockquote class="twitter-tweet" data-lang="ja"><p lang="en" dir="ltr">Angular 2 final release progress now at 20%.<br>This can regress if we find critical issues, but GO TEAM GO! :) <a href="https://t.co/R7EExZOpTE">pic.twitter.com/R7EExZOpTE</a></p>&mdash; Brad Green (@bradlygreen) <a href="https://twitter.com/bradlygreen/status/709521449070022657">2016年3月14日</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

### Angularプロジェクトのnpmガイドライン
Angularプロジェクトで公開するnpmパッケージについてルールを決めようという話です。
現在Angular2本体は`angular2`として公開されていますが、npmで検索した時にどれが公式のパッケージなのかわからない問題があるということでどうにか解決できないかと模索中です。
具体的には、先日公開されたng1用のComponentRouterは`@angular/router`として公開されており、`@angular/`スコープを一律で使うようにするのがいいんじゃないかという意見が強いです。
ただし特殊なパッケージ名に見えるしnpm依存の何かしらのツールでは対応できないんじゃないかという懸念がありより良い方法がないか検討中です。

### Fluent Conferenceが開催されました
オライリーが主催のFluent Conferenceが開催され、BradがKeynoteでAngular 2についてスピーチしました。

[Angular 2 and the future of HTML5 apps - O'Reilly Media](https://www.oreilly.com/ideas/angular-2-and-the-future-of-html5-apps)

先程のCode Generationの画像もこの動画からです。Angularイベントではないのに新情報が盛り沢山なのですが、一番衝撃的だったのはAngular Universalの今後です。
現在Node.js用に開発されているAngular Universalが将来的には.NetやPHP、Javaでも使えるようにするとの話です。

![Imgur](http://i.imgur.com/Dp3zMzm.png)

先日JeffがJavaやPythonもサポートするだろうと言っていたのはあまり真に受けてなかったのですが、Bradが公式にプレゼンした以上これはほぼ確約と言っていいのではないかと思います。
とはいえ今年中に来る気はしないですが楽しみに待ちましょう。

### ng1用ComponentRouterのドキュメントができました
Angular 1.5のドキュメントにComponent Routerの使いかたが追加されました。

[AngularJS: Developer Guide: Component Router](https://docs.angularjs.org/guide/component-router)

### Zonesの標準化
Zonesの仕様をTC39のStage 1に上げようという計画です。現在のZone.jsの全部の機能を仕様化するのはやめて、最小限の機能でまずはv1としてStage 1に移行させる予定です。
今月28日のTC39のミーティングにむけて作業中のようです。

### メソッドパラメータのDecoratorについて
Babelでメソッドの引数にDecoratorを使えるようにPRを出したらしいです。これが通ればAngular 2のES6版とTS版は型の有無以外の差がなくなります。

----

ざっくりこんな感じです。Bradの動画はぜひ見て欲しいです。

今日はさらに今週リリースされるであろうBeta.10の予告も行います。久々に大きな変更がありますので備えておきましょう。

### `@View`の廃止 [破壊的変更]
随分前から非推奨になっていた`@View`アノテーションがついに削除されました。もし古いコードで依存している場合は`@Component`に切り替えましょう

[chore(core): remove @View annotation · angular/angular@f9fb72f](https://github.com/angular/angular/commit/f9fb72fb0e9bcbda7aeebbf8321ce5d70d78ecee)

### Shadow CSSにおける`/deep/`と`>>>`のサポート
CSSの`/deep/`と`>>>`が`ViewEncapsulate.Emutatedでもサポートされます

[feat(shadow_css): support `/deep/` and `>>>` by tbosch · Pull Request #7563 · angular/angular](https://github.com/angular/angular/pull/7563)

### `ngPlural`の追加
i18n用の新しいディレクティブ`ngPlural`と`ngPluralCase`が追加されました。`ngSwitch`と似たような使い方をします。

```
@Component({
  selector: 'app',
  template: `
    <div [ngPlural]="value">
      <template ngPluralCase="=0">there is nothing</template>
      <template ngPluralCase="=1">there is one</template>
      <template ngPluralCase="other">there is some number</template>
    </div>
  `,
  directives: [NgPlural, NgPluralCase]
})
 ```

----

というわけで今回はここまで。それではまた。
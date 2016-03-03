title: Beta.8がリリースされました
date: 2016-03-03 21:08:12
author: laco0416
tags:
- Release
- Beta
- ComponentRouter
- BreakingChanges
- Router

---

どうも、らこです。予想通り今週はBeta.8がリリースされましたので、主な変更点をさらっていこうと思います。

## ComponentRouter(Angular 1用)の破壊的変更
Angular 1向けのComponent Routerに破壊的な変更が入りました。DIの文字列が大きく変わっています。
`$route`から`$rootRouter`に、`router`から`$router`になっているので、要注意です。
また、`templateUrl`を持つcomponentにルーティングできなかったバグが修正されています。

- [fix(angular1_router): rename `$route` service to `$rootRouter` · angular/angular@a1c3be2](https://github.com/angular/angular/commit/a1c3be2)
- [fix(angular1_router): rename `router` component binding to `$router` · angular/angular@edad8e3](https://github.com/angular/angular/commit/edad8e3)
- [fix(angular1_router): support templateUrl components · angular/angular@d4a4d81](https://github.com/angular/angular/commit/d4a4d81)


## `trackBy`のバグ修正
`ngFor`の`trackBy`でインデックスを使った際に配列の操作で容易にChange Detectorが壊れるバグが修正されました
いろいろ危なっかしい実装だったので心配してましたがようやく安心して`trackBy`できます。

- [fix(differ): clean up stale identity change refs · angular/angular@ab36ea0](https://github.com/angular/angular/commit/ab36ea0)

ちなみにAngular 1でもAngular 2でも共通する話ですが、`ng-repeat`も`ngFor`も`trackBy`を使うと目に見えてパフォーマンスが上がります。
配列の変更検知をするためには「どの要素」が変わったのかを効率よく調べないと全要素舐め直すことになるので、
一意性の調べ方をこちらで提供してあげるとめっちゃ早くなります。前にも貼った気がしますが、
[Is Angular 2 faster ? · Issue #7088 · angular/angular](https://github.com/angular/angular/issues/7088)
では最初に「`track by`した`ng-repeat`」と「素の`ngFor`」を比較してAngular 2が本当に速いのか疑問を投げかけていますが、
[`ngFor`と`trackBy`を併用](https://plnkr.co/edit/cjFGtnI704bjSg6F0DEM?p=preview)すると明らかにAngular 2のほうが速くなっていることがわかりました。

要素数が多く、変更も多いパフォーマンスのネックになりがちな部分には`trackBy`をつけるようにしましょう。

## `QueryList#forEach()`の実装
TypeScript/JavaScript側の`QueryList`にはなかった`forEach`メソッドが追加されました。ちなみに私が実装しました。

- [feat(core): Add `QueryList#forEach` · angular/angular@b634a25](https://github.com/angular/angular/commit/b634a25)
- [feat(core): Add `QueryList.forEach` to public api. · angular/angular@e7470d5](https://github.com/angular/angular/commit/e7470d5)

Dart版では`QueryList`は`IterableMixin`クラスを継承しているのでそのまま`forEach`が暗黙のうちに生えていたのですが、
TS/JSでは単なるクラスなので、APIに差ができてしまっていました。

- [angular/query_list.ts at d4a4d81173ff31ab8af0f5928735399d92d73339 · angular/angular](https://github.com/angular/angular/blob/d4a4d81173ff31ab8af0f5928735399d92d73339/modules%2Fangular2%2Fsrc%2Fcore%2Flinker%2Fquery_list.ts)
- [angular/query_list.dart at d4a4d81173ff31ab8af0f5928735399d92d73339 · angular/angular](https://github.com/angular/angular/blob/d4a4d81173ff31ab8af0f5928735399d92d73339/modules%2Fangular2%2Fsrc%2Fcore%2Flinker%2Fquery_list.dart)
- [IterableMixin class - dart:collection library - Dart API](https://api.dartlang.org/1.14.2/dart-collection/IterableMixin-class.html)

## デバッグ用のAPIが新しく追加された
デバッグ用のAPIが3つも増えました。

- `window.getAllAngularRootElements()` : ページ上で`angular.bootstrap()`の対象となっているルートエレメントを取得できる
- `ng.coreTokens.ApplicationRef` : `ApplicationRef`の参照が取れる
- `ng.coreTokens.Ngzone` : `NgZone`の参照が取れる

`ng.prove`と合わせたこれらデバッグAPIの使い方は近いうちに別に特集しようと思います。

- [feat(core): add more debug APIs to inspect the application form a bro… · angular/angular@b5e6319](https://github.com/angular/angular/commit/b5e6319)

## Dart版でジェネリクスを使ったDIの廃止
Dart版のみで、ジェネリック付きの型がDIできなくなりました。背景にはOffline Template Compiler周りの実装の障害になったことがあるようです。

- [feat(di): drop support for injecting types with generics in Dart · angular/angular@c9a3df9](https://github.com/angular/angular/commit/c9a3df9)

## `pattern`バリデータの追加
ビルトインのバリデータに`pattern`バリデータが追加されました。`input`要素のバリデーションに正規表現でパターンを指定できます。

```
<input [ngControl]="fullName" pattern="[a-zA-Z ]*">
```

- [feat(forms/validators): pattern validator · angular/angular@38cb526](https://github.com/angular/angular/commit/38cb526)

## i18n機能のPipeの追加
i18n用に新しく`i18nPlural`と`i18nSelect`の2つのPipeが追加されました。

### `i18nPlural`
`i18nPlural`は数値に関するi18n対応を担います。数値の値によって文字列の単数形、複数形を対応させる際に使えます。

```
<div>
  {{ messages.length | i18nPlural: messageMapping }}
</div>

class MyApp {
  messages: any[];
  messageMapping: any = {
    '=0': 'No messages.',
    '=1': 'One message.',
    'other': '# messages.'
  }
  ...
}
```

### `i18nSelect`
`i18nSelect`は文字列に関するi18n対応を担います。文字列の値によって表現を変える部分に有用です。
次の例では`gender`の値によってテキストの中の`her`や`him`を切り替えています。

```
<div>
  {{ gender | i18nSelect: inviteMap }}
</div>

class MyApp {
  gender: string = 'male';
  inviteMap: any = {
    'male': 'Invite her.',
    'female': 'Invite him.',
    'other': 'Invite them.'
  }
  ...
}
```

- [feat(i18n): added i18nPlural and i18nSelect pipes · angular/angular@59629a0](https://github.com/angular/angular/commit/59629a0)

## `replace` Pipeの追加
文字列を置換する`replace` Pipeが追加されました。

```
{{ expression | replace:pattern:replacement }}
```

という形式で記述できます。具体的には次のようになります。

```
<div>
  {{ 'abcdef' | replace:abcPattern:'ABC' }}
</div>

class MyApp {
  abcPattern = /abc/g;
}
```

実行すると `ABCdef` という風に置換されます。挙動は`String.prototype.replace`と互換があります。

- [feat(pipes): add ReplacePipe for string manipulation · angular/angular@6ef2121](https://github.com/angular/angular/commit/6ef2121)


----

今週は便利なAPIがたくさん増えました。こういうアップデートは久々なので良いリファクタリングの機会かもしれません。

ちなみにBeta.9のリリースからはいよいよrouterの改革が始まります。`routerLink`のDSLが徐々に廃止され、正規表現と関数によってルーティングが記述できるようになります。

- [feat(router): add regex matchers · angular/angular@75343eb](https://github.com/angular/angular/commit/75343eb34007579be9cdc803da834c38e02ae12c)

破壊的変更にはしないらしいのでゆったり構えておきましょう。

参考資料

- [angular/CHANGELOG.md at master · angular/angular](https://github.com/angular/angular/blob/master/CHANGELOG.md#200-beta8-2016-03-02)
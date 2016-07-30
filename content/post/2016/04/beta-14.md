+++
title = "Beta.14がリリースされました"
date = "2016-04-09T00:12:23+09:00"
tags =["Beta","Release"]
+++

どうも、らこです。今週もベータリリースが行われ、Beta.14となりました。今回は殆どがバグ修正ですが、変更点をチェックしましょう。

<!--more-->

---

### upgrade: Angular 1のディレクティブ破棄イベントが取得できなかったバグを修正
[fix(upgrade): leak when angular1 destroys element · angular/angular@9be04f8](https://github.com/angular/angular/commit/9be04f8)

UpgradeAdapterを使ってダウングレードしたコンポーネントが、正しく破棄イベントを拾えていなかったバグが修正されました

### form: select要素のvalueにオブジェクトをバインディングできるように修正
[fix(select): support objects as select values · angular/angular@74e2bd7](https://github.com/angular/angular/commit/74e2bd7)
[fix(forms): support both value and ng-value · angular/angular@8db97b0](https://github.com/angular/angular/commit/8db97b0)
[fix(select): update name from ng-value to ngValue · angular/angular@3ca6df8](https://github.com/angular/angular/commit/3ca6df8)

いままではselect要素の中のoption要素はvalue属性に文字列しか渡せませんでしたが、オブジェクトをデータバインディングできるように修正されました。

```html
<option [ngValue]="obj">{{obj.text}}</option>
```

### router: クエリパラメータにスラッシュが含まれている場合に起こる不具合を修正
[fix(router): allow forward slashes in query parameters · angular/angular@4902244](https://github.com/angular/angular/commit/4902244)

クエリパラメータ中にスラッシュが存在するのを許容するようになりました。

---

この他にもOffline Template Compilationに向けた内部的な機能追加やリファクタリングが進んでいます。ng-confまで1ヶ月を切ったのでこれからどんどんブラッシュアップが進むでしょう。
routerのリファクタリングも待っているので要チェックです。




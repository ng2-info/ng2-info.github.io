+++
title= "Angular2 RC.3の変更点"
date= "2016-06-29T00:25:32+09:00"
tags = ["RC","Release"]
+++

どうも、らこです。リリースから少し時間が経ってしまいましたが、RC.3の変更点をまとめておきます。

<!--more-->

[CHANGELOG.md](https://github.com/angular/angular/blob/master/CHANGELOG.md#200-rc3-2016-06-21)

---

### Bug Fixes

RC.2までの幾つかのバグが修正されました。重要なものだけをピックアップします。

* **change_detection:** ChangeDetectorRef reattach should restore original mode ([773c349](https://github.com/angular/angular/commit/773c349)), closes [#7078](https://github.com/angular/angular/issues/7078) [#7080](https://github.com/angular/angular/issues/7080)

`ChangeDetectorRef.reattach`が常に`CheckAlways`を使用していたバグが修正されました。

* **forms:** ngModel should export as ngModel ([8e6e90e](https://github.com/angular/angular/commit/8e6e90e))

`NgModel`ディレクティブが`ngForm`としてexportされていたのが修正されました。

* **perf:** support prod mode again ([c0f2a22](https://github.com/angular/angular/commit/c0f2a22)), closes [#9318](https://github.com/angular/angular/issues/9318) [#8508](https://github.com/angular/angular/issues/8508) [#9318](https://github.com/angular/angular/issues/9318)

RC.2でうまく動かなくなっていた`enableProdMode`が復活しました。


### Features
新しい機能や改善も幾つか含まれています。

* **compiler:** make interpolation symbols configurable (`@Component` config) (#9367) ([1b28cf7](https://github.com/angular/angular/commit/1b28cf7)), closes [#9158](https://github.com/angular/angular/issues/9158)

`Component`デコレータに`interpolation`プロパティが追加され、そのコンポーネントのテンプレート中でのInterpolationのシンボルが変更できるようになりました。
この機能は私が実装して、@vicbがしっかりレビューしてくれました。
merge前のちょっとしたミスで私のcommitではなくなってしまいましたがそれはご愛嬌ということで。

* **datePipe:** numeric string support ([5c8d315](https://github.com/angular/angular/commit/5c8d315))

`DatePipe`が文字列型の数値を受け付けられるようになりました。

* **QueryList:** support index in callbacks ([5fe6075](https://github.com/angular/angular/commit/5fe6075)), closes [#9278](https://github.com/angular/angular/issues/9278)

`QueryList`の各メソッドのコールバックで`index`が第2引数に追加されました。

* **radio:** support radio button sharing a control ([39e0b49](https://github.com/angular/angular/commit/39e0b49))

ラジオボタンに対する`ngModel`が使いやすくなりました。
複数のラジオボタンに対して同じオブジェクトを`ngModel`で渡した時に、選択状態のラジオボタンのvalueがセットされるようになりました。

今までは配列やオブジェクトなどを用いて面倒でしたが、Angular 1と似た書き方になり、とても簡単になりました。

```html
<form>
    <input type="radio" name="food" [(ngModel)]="data.food" value="chicken">
    <input type="radio" name="food"  [(ngModel)]="data.food" value="fish">
    <input type="radio" name="drink" [(ngModel)]="data.drink" value="cola">
    <input type="radio" name="drink" [(ngModel)]="data.drink" value="sprite">
</form>
```


### BREAKING CHANGES
わずかな破壊的変更も含まれています。

* Parse5Adapter is no longer exported as public API, use serverBootstrap()

`Parse5Adapter`はこれまでパブリックなAPIでしたが、RC.3からはexportされなくなりました。
代わりに`serverBootstrap()`関数を呼び出します。


---

以上、RC.3の変更点でした。RC.2でパフォーマンスの問題を発生させてなければもう少し先になっていたと思いますが、
思っていた以上に深刻な不具合だったので、だいぶ前倒しでRC.3がリリースされました。
RC.4は予定通り、FormsやTestingのAPIの改善が完了してからの落ち着いたリリースになるでしょう。

それではまた。
title: Component Router 解説スライドの紹介他
date: 2016-03-06 22:25:04
author: laco0416
tags:
- Router
- Event

---

どうも、らこです。今日は[@gerardsans](https://twitter.com/gerardsans)によるAngular 2のComponent Routerについてのスライドを紹介します。

## Angular 2 Component Router (@gerardsans)

<iframe src="//slides.com/gerardsans/riga-dev-day-component-router/embed" width="576" height="420" scrolling="no" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

[スライド](http://slides.com/gerardsans/riga-dev-day-component-router#/)

### Component Routerの主な機能

- コンポーネントをベースにしている
- 柔軟なルーティング
  - 入れ子のビュー、複数ビュー
- ライフサイクルフック
- 遅延読み込み

![Imgur](http://i.imgur.com/8i4fQqG.png)

[bit.ly/spiderman-app](bit.ly/spiderman-app)

### セットアップ
Component Routerを使う上で依存するもの

- Angular 2に同梱されているrouter.js
- LocationStrategyの選択
- `ROUTER_PROVIDERS`
- `ROUTER_DIRECTIVES`

#### LocationStrategy
Component RouterのルーティングがURLでどう表現されるかを選択できる。

- HashLocationStrategy: #/home, #/users/34
- PathLocationStrategy: /home, /users/34 (`APP_HREF_BASE`が別途必要)

#### ルートの定義

Componentに対して`RouteConfig`アノテーションでルートを定義する。

![Imgur](http://i.imgur.com/JEiycGX.png)

#### アウトレットの宣言
Component RouterでルーティングされたComponentが読み込まれる場所を`<router-outlet>`要素で宣言する。

![Imgur](http://i.imgur.com/fX35h0a.png)

### 柔軟なルーティング

#### 入れ子ルーティング
親側のComponent側で、`/...`を使ったルートを定義すると、その対象のComponent側で入れ子になったルートを定義できる。
子側のComponentでは必ず`useAsDefault`に設定されたルート定義が必要である。

次の例では親のルート定義で`/users/...`に対して`Users`が割り当てられた時の、子のルート定義の例を示している。
`/`に対するルートは`useAsDefault`になっており、`/users`と`/users/`どちらにもヒットする。

![Imgur](http://i.imgur.com/UTZUPaA.png)
![Imgur](http://i.imgur.com/Uan3ctq.png)


#### ナビゲーション
- 選択したLocationStrategyによってURLの表現は変わる
- `routerLink`ディレクティブではLink DSLを使ってルートを表現する
- プログラム上では`Router`クラスを使ってナビゲーションできる

![Imgur](http://i.imgur.com/Uan3ctq.png)

#### 複数ルーティング(Auxルーティング)
- `path`ではなく`aux`を指定したルート定義は、対応した`name`が設定された`router-outlet`に作用する
- 複数の`router-outlet`について、柔軟にナビゲーションが可能

![Imgur](http://i.imgur.com/Miq0dMx.png)

#### パスパラメータの取得
`:id`など、パス中のパラメータは`RouteParams`としてDIで取得できる

![Imgur](http://i.imgur.com/PF9haKF.png)

### ライフサイクル
- `CanDeactive`フックを使うと画面遷移前の保存確認ができる
- `CanActivate`フックを使うとアクセス制御ができる

![Imgur](http://i.imgur.com/6rMvGSF.png)

### 遅延読み込み
`component`の代わりに`loader`を設定するとルーティングが発生するまでComponentの読み込みを遅延できる

### デモ
一連の内容は[Plunker](http://plnkr.co/edit/PdQTp0ZirG6BOtk2uj16?p=preview)で動かすことができる

----

ざっくりと重要な部分だけを要約しました。時間がある方はスライドを見ながらデモを動かすとRouterをばっちり理解できるはずです。

----

## ng-sakeやります
突然ですが、私が主催するイベントの紹介です。ng-sakeというAngularユーザ向けのイベントを開催します。

[ng-sake](http://connpass.com/event/27707/)

ng-sakeはお酒を飲みながらAngularについて話すイベントです。開催日は3/30で、ng-japanの翌週です。
ng-japanのアフターフォローも兼ねた会にしようと思っていて、ng-japanでも一緒に登壇する@armorik83にも手伝ってもらう予定です。Angularに関する大体の質問には答えられるはずです。

募集開始は **3/8(火)の朝10時** です。第1回なのもありどれくらい集められるのかわからないので枠が20人と少なめです。
もし興味があれば忘れないように申し込みしていただければ幸いです。

それでは。
+++
date = "2016-08-06T11:26:50+09:00"
tags = ["Event"]
title = "ng-sake#5を開催しました"

+++

どうも、らこです。8/3にng-sakeというイベントの第5回を開催しましたので、そのレポートです。

[ng\-sake \#5 \- connpass](http://ng-sake.connpass.com/event/36225/)

<!--more-->

今回の参加者は10人で、なんと欠席は1人だけでした。用意したお酒が無駄にならなくてよかったです。

以下はディスカッションの内容のざっくりしたまとめです

- Animationについていくつか質問があります。メトロノームに前回アドバイス頂いた部分を反映しましたので、その報告と新たな質問（Animation）をさせて下さい。

kawakamiさんが作っているメトロノームアプリの相談。
前回の相談でコンポーネントツリーの構造が改善されたので、次はアニメーションを導入してみた話。

- Atomic Designについて雑に話したい

Angular2と[Atomic Design](http://bradfrost.com/blog/post/atomic-web-design/)は微妙に噛み合わないなという話をした。
Angularでコンポーネントを作ろうと思う粒度はせいぜいMoleculeで、ヘタするとOrganismで1コンポーネントもあり得る。
実装からコンポーネントを考えるとだいたいトップダウンで、再利用できそうなところを分割していくけど、
Atomic Designはボトムアップだから、アプリケーションの開発には向いてないなという感じに落ち着いた。

- Native Apps vs Web Apps　みなさんどのように考えているか聞きたいです

よく言われるが、リーチはWebが強いので入り口に使おうという感じ。
あと、ネイティブアプリだとスプラッシュスクリーンで起動時間をごまかすのがあまり忌避されないのがズルい。

- Custom Form Controlsの使いどころ。 http://blog.thoughtram.io/angular/2016/07/27/custom-form-controls-in-angular-2.html

アプリケーション作者のためのものじゃなくてコンポーネントライブラリ作者向けだという結論

- RoutingのGuardの具体的な使い道。

編集画面からの保存前離脱チェックや、認証チェックなど。実装としてはただのServiceなのでDI経由で何でも使えて自由に使える

- サーバークライアント両方やる案件として、チームでどういう人数比率が一般的なのか気になる

だいたいフロントが少数で、サーバーのほうが人数が多いようだった。

- ActivatedRouteのparamsをsnapshotではなくObservableとして受け取らなければならないケースにどういったものがあるか。

同じルートに遷移する場合。 `/user/11` から `user/12` に遷移する時はコンポーネントが再利用されるので動的にパラメータを受け取らないといけない。

- アニメーションをCSSでやるのかAnimationAPIでやるのか。最近のトレンドを踏まえつつ、これからのアニメーション作り方、これら使い分けについて有識者の話を聞きたいです。

残念ながら有識者はいなかったけど、Angular2でやるならWeb Animation APIがよさそう。
Specはちょうど8月頭にドラフトが更新されてた。

https://w3c.github.io/web-animations/

- そろそろ案件で使いたいのでプロジェクトのScafoldについて。現実解を知りたいです。

みんな好きにやってる。yo使ってる人は全然いなかった。yeoman-generatorを作るのがつらい話もあった。

- Angular-CLIがwebpack対応したらしいけど誰か試しました？

誰も試してなかった。

- rc.5からAnimation APIでページ遷移のアニメーションができるようになるという話ですが、そもそもページ遷移にアニメーションを導入したいかしたくないか。またはすべきかすべきでないか。

みんなやりたかった。

- ng2にもng-cloak的な仕組み欲しい気がする

必要ないのでは？という雰囲気。

- このイベントの名前もうちょっと良い感じのないかなと思いつつある。Angular酒部とか、井戸端Angularとか ご意見求む

ng-sakeでいいよという結論

- ここ2回くらいはLTタイムを無くしているけど、LTがあるイベントのほうが行きたいものですか？

次回2枠くらい復活させてみようと言う話になった

- WebComponents ベースのライブラリを Angular 2 で wrap しようとするとカオスになる問題の解決策について

Custom ElementsとAngular 2の相性の悪さについて。RC.5で追加される機能で解決する問題だった。

- Angular2にむけて(そうじゃなくても)TypeScriptを最近始めた人多いと思うけど、何を教材にしました？

公式のハンドブックを読みながら覚えた人が多かった。公式のチュートリアルはあまりよくなかったらしい。

- Angular(1.xでも2.xでも) Component Guide(Style Guide)を作るとしたら、どんな方法が良いのでしょう？ dgeniは一度爆死したことがあるので、使いたくありません。

１つそれ用のアプリを作るしかないよねという結論。
Angularチームも[periscope](https://github.com/angular/periscope)というアプリケーションでドッグフーディングしているし、
Material2もデモアプリを同梱している。

- アーキテクチャどうしてますか？

データフロー的な話。一切Component内に同期的なステートを持たずに、すべてをSubjectにするみたいなやり方はすごいけど原理主義っぽかった。

- SPA全般ですが、戻るボタンで前のページに戻ったときなどに、ページが再構築されてしまうのが微妙だなと思うのですが、なにか対策してますか？ APIのリクエスト結果をキャッシュするとか？

スクロール位置とか、ページングとかの話。URLにシリアライズできるならルーティングをちゃんとやって、
それ以外はlocalStorageとかsessionStorageで持つしかないよねという感じ。
他にもモーダルで出してしまえばそもそも元の画面を捨てずに済むよねというアイデアも。

- 新しいプロジェクトでAngular2かReactで迷ってますが、なにか選定のポイントなどアドバイスがあれば聞きたいです。

結局人による。選べるならやりたい方やればいい。モックアプリみたいなものを両方で実装してみて感触がよかったほうでいいんじゃないという声も

## 関連リンク

ディスカッションの中で登場したリンクなど

- [Presentational and Container Components — Medium](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0#.hyn6o5osb)
- [Angular2\(rc\.4\)のRouting, Formsの簡単なサンプル。ついでにFlux。 \- Qiita](http://qiita.com/ovrmrw/items/28e5384771925b46b891)
- [Visual Studio Code \- Code Editing\. Redefined](https://code.visualstudio.com)
- [Basic Types · TypeScript](http://www.typescriptlang.org/docs/handbook/basic-types.html)
- [angular/material2: Material Design components for Angular 2](https://github.com/angular/material2)
- [laco0416/angular2\-tour\-of\-heroes: Tour of Heroes with the latest API](https://github.com/laco0416/angular2-tour-of-heroes)

----

次回は9月か10月にやる予定です。
今回はプレミアムモルツを入荷して好評だったので、次回ももしかしたら用意するかもです。日本酒とかワインも飲みたいですね。

参加者のみなさん、ありがとうございました！
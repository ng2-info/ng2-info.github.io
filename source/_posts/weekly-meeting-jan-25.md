title: 1/25 週次ミーティング議事録
date: 2016-01-26 10:11:27
author: laco0416
tags:
- Meeting
- Planning
- Router
- AngularJS
- Animation
- Mobile
- Release
---

こんにちは。今週のAngularチームの週次ミーティングについての情報です。
今後のリリースや計画について多く言及されているので要チェックな内容です。

議事録は[こちら](https://docs.google.com/document/d/150lerb1LmNLuau_a_EznPV1I1UHMTbEl61t4hZ7ZpS0/edit#)

---

### Demo of the new Dart Dev Compiler for the team
Angularチーム内でDDCを使った開発のデモが行われたようです。
DDCはDartの新しいコンパイラで、特徴はヒューマンリーダブルなES6を吐き出すことです。
DDCによる開発をサポートすることで、Angular2 for Dartユーザの開発サイクルを改善できるとしています。

DDCがサポートされるとDartium以外のブラウザでも即時コンパイルが可能になり、
さらにDDCが吐き出したES6はBabelなどのコンパイラでさらにES5に変換可能とのこと。
DDCがどれほど安定しているのかはさておき、for Dartのユーザには朗報です。

### Update on i18n support
[先日取り上げた](2016/01/16/working-about-i18n-in-jan/)Angular2のi18n対応についての続報です。

i18n対応の目標は`{% raw %}{{}}{% endraw %}`によってテンプレートを多言語化することであると改めて定められ、
まずはじめに静的な文字列について翻訳できるように翻訳できるように作業中とのこと。
将来的には性別や単数・複数形などの動的なシナリオについてもサポートする予定です。

忙しくて忘れられているのではないかと思われていましたが、ちゃんと取り組んでいて安心しました。

### Router update
Routerモジュールのアップデートが行われます。
遅延ローディングや、コンポーネントとルーティング設定のコードの分割を可能にするための修正が行われる予定で、
この変更のために現在のRouterのシンタックスが多少変わる可能性があります。

ただし今後数週間かけて改修を行うとのことで、今すぐに破壊的変更が来ることはないでしょう。

### Angular 1.5 update
AngularJSの1.5 RC2が水曜日（日本時間だと木曜日）にリリース予定です。
そして、1.5の最終版は来週、遅くても再来週にはリリースするとのこと。

1.5 finalに向け、Brian氏はAngular2のComponent Routerと協調できるように作業中らしく、
`component`の導入によってAngular2と同じスタイルでComponent Routerを使って開発できるようになるようです。

1.5 finalのリリース時には`component`の使い方についてのチュートリアルやドキュメントを整備するらしく、
まだAngular2に触れていないAngularJSユーザでもスムーズにコンポーネント化を進めることができるでしょう。

### Animation Update
Animationサポートについても情報の更新がありました。
Animationモジュールのデザインドキュメントが[公開](https://docs.google.com/document/d/1rTJKNJ6Rv2N5jElLIrrukuEsS_kFOQPH1D5O1Dfva44/edit#heading=h.x01aekc0f0sk)され、
CSSと協調したアノテーションベースのアニメーション記述を可能にする方針であることがわかりました。
また、アニメーションにはW3Cで仕様策定中の[Web Animations API](https://www.w3.org/TR/web-animations-1/)を使用するようです。

現在はCSSを解釈するためのパーサが開発中で、まもなく完成します。
[残りのタスク](https://docs.google.com/spreadsheets/d/1G5-hb4DpqddLtDV0PD0cueeVh7Eb8o7fPmNryX4AvGM/edit#gid=0)はまだたくさんありますが、
すでに半分は作業中なのでパーサさえできてしまえば案外早くBetaに入ってくるのではないでしょうか。

### Mobile update
モバイル対応についての情報も追加されました。

リアルタイムDBのFirebaseをAngularJSから使うライブラリとしてAngularFireというものがありますが、
Angular2用のAngularFire 2のアルファ版が近々公開される予定です。

また、Angular2を使ったProgressive Web Appのリファレンスとなるアプリケーションを開発中で、
サーバサイドレンダリングやService Worker、Web Worker、HTTP/2などの要素を盛り込むとのこと。

### Angular 2 release plan
最後に、今後のAngular2 Betaのリリースに関する情報です。
最近はBeta版で落ち着いていた印象ですが、今週、遅くても来週あたりからはAlphaの頃と同じく
毎週リリースを行う計画です。
次のリリースから最終版までずっとそのペースで行くつもりらしく、開発陣の気合の入りようが伺えます。

---

先週の週次ミーティングがなかった分、盛りだくさんの内容になりました。

おまけですが、AngularConnect 2016のWebサイトが公開されました。

[Home page | AngularConnect - The Official European Angular Conference 2016](http://angularconnect.com/)

今年は9月の26日、27日にロンドンで行われます。
9月にはもうRC版になっているのか、まだBetaなのか、はたまたもうfinalになっているのかわかりませんが
去年のような充実したイベントになるのは間違いないでしょう。とても楽しみですね。

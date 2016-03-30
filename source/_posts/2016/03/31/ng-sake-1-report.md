title: ng-sake を開催しました
date: 2016-03-31 00:48:45
author: laco0416
tags:
- Event

---

どうも、らこです。3/30に[ng-sake #1](http://connpass.com/event/27707/)を開催しましたので、そのレポート記事です。

![Imgur](http://i.imgur.com/nPB5WHK.jpg)

第1回だったのでいろいろと実験的な試みもありましたが、概ね好評だったようですし私も満足しています。
今後も毎月くらいのスパンで開催できたらいいなと思っています。

## 趣旨
「Angular好きで集まって酒飲みながら話をしよう！」という簡単な趣旨でした。
前の週にng-japan 2016が開催されたのもあり、Angularコミュニティが活性化していたのでその勢いに乗じる形でした。

## 内容

開始と共に乾杯しました。

### LTパート

前半は事前に募集したLTの発表時間です

#### @Quramyさん: SSR with Angular 2

![Imgur](http://i.imgur.com/bSEt4EQ.jpg)

<script async class="speakerdeck-embed" data-id="8b4fd4950fcd42709d38fa585d3199bd" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

Angular 2のサーバーサイドレンダリングに関する発表でした。実際に[angular/universal](https://github.com/angular/universal)を使ったデモや、
さらにはpreloadまで試したデモを見せてもらえてとても新鮮でした。

#### @mitsuruogさん: Angular 2 Unit testing Overview

![Imgur](http://i.imgur.com/BNrpEYJ.jpg)

<iframe src="//www.slideshare.net/slideshow/embed_code/key/5eDmVzNYiE0Jl6" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/mitsuruogawa33/angular-2-unit-testing-overview" title="Angular 2 unit testing overview" target="_blank">Angular 2 unit testing overview</a> </strong> from <strong><a target="_blank" href="//www.slideshare.net/mitsuruogawa33">Mitsuru Ogawa</a></strong> </div>

Angular 2のユニットテストに関する発表です。`angular2/testing`パッケージを使ってテストを書くときの具体的なサンプルコードがたくさん紹介されました。
Angular 2のドキュメントの中でもテスト周りは特に貧弱なので、まとまった資料があるのはとてもありがたいですね。

#### @tyfkdaさん: Angular2 触ってみた

![Imgur](http://i.imgur.com/5wOPHr7.jpg)

[スライドURL](https://docs.google.com/presentation/d/1GWgqxAxRNjaC5FrF0x4XGwmdZlV8LlE2feA2SGUVVy0/edit#slide=id.p)

Angular 1の機能とそれに対応するAngular 2の書き方の紹介や、Angular 2を実際に書いてみてすごいと思った点などが挙げられました。
Angular 2とFirebaseを合わせたアプリケーションのデモもありました。

#### @norami_dreamさん
詳細は公開NGとのことですが、Angular 1のSPAでUIを重視したアプリケーションを作る工夫や、CSSフレームワークの衝突の回避方法などを話してもらいました。

### ディスカッションパート
後半はNode Discussion形式で、 sli.doを使ってテーマを募ってディスカッションしました。
当初は30分くらいのつもりでしたが気づいたら1時間半ほどみんなでディスカッションしていたので詳細は覚えていないのですがざっくりとまとめます。

#### Angular 2やコンポーネント志向の開発におけるテストファイルの置き場所
`app/app.spec.ts` vs `test/app/app.spec.ts` 

Angular 2のコードスタイルガイドでは前者が取り上げれているけど、会場では後者のほうが多かったです。
理由としてはkarmaなどの設定ファイルでテスト用のコードのパターンが書きやすいとか、テスト用のtsconfig.jsonが配置できるとかありました。
結論は「チーム内で決めろ」でした

#### ES2015があるのになぜTypeScriptなのか？
結論「型とデコレータとジェネリクス」

#### gulpを使わずにnpm scriptsを使うべきか
会場での集計ではnpm scripts派が2人、Gulp派が7人、Grunt派が3人くらいで、Gulpは全員がv3系でした。
結論は「規模による」

#### `$rootstrap.$broadcast()` のng2移行手順が確立されてない！
会場でAngular 2移行作業してるのが私と@armorik83(出題者)の2人だけだったので流しました。

#### デザイナとの協業について
デザイナにどこまで仕事をお願いしているかなど結構話が盛り上がりました。
結論としては「PSDだけください」に落ち着いたみたいです。

#### Rxの詳細（使われ方／使い方）などを知りたい
すでにQiitaやその他媒体にRxの記事はあるので、読むといいよという話になりました。
Rxの根幹の部分は本家Rx(C#)やRxJavaなどにけっこう知見が詰まっているので資料はけっこうあります。
ただRxJSはv4とv5でかなり違うのでドキュメントの選別が難しいという話もありました。

#### TypeScriptをWebStormで書くおすすめの設定
WebStormの設定を詰めてる人はあまりいなかったようです。ほぼデフォルトで使っている人が多かったみたいです。
VS Codeやその他エディタ勢からの攻撃もありました。

#### WebpackとSystemJS
開発・プロダクションでどちらをどう使うべきかという話。SystemJSはまだ知見がなくてちゃんと使ってる人がほとんどいなくて、
とりあえず今はWebpackのdevserverを活用して開発もbundleもやるのがいいんじゃないかくらいのところに落ち着きました。

SystemJSはコンフィグがまるでわからんという話で、近々AngularチームのrobwormaldがSystemJS設定に関するブログを書くはずなのでそれを待ちましょう。

#### Angular2でのCSS管理、設計の話
Angular 2だとデフォルトでCSSにスコープがあるけど、それに乗っかるべきかグローバルで今までどおり戦うべきか、
乗っかるとしてコンポーネント内部でのクラス名の付け方とかどうするべきかという踏み込んだ話。
Angular 2の仕組みに乗っかる恩恵のほうが大きいので、スコープを享受する方向で、
小分けになったCSSの中では要素名とか汎用的なクラス名で指定していいのでは？という流れになった。

複雑なCSSが発生するなら、それはディレクティブやコンポーネントに切り出すことで単純化できそう、Angular Material 2の`md-button`みたいな仕組みがよいかもという話をしました。

#### Angular 1のパフォーマンス向上のよくあるネタ
結論「ディレクティブを減らす、watchを減らす」

#### ReactからAngular 2への移行難易度が知りたい
結論「試して記事を書いてくれ」

#### Angular 2とSass
コンポーネントごとに1つSassのファイルがある構成で、個別にコンパイルすればいいんじゃない？ということになりました。

余談ですがCompassが実はいつのまにかLibsass依存になってたらしく、rubyのsassを使ってないということを今日始めて知りました。

#### ng-showないとCSSで隣接セレクタとか使うときつらい
Angular 2に`ng-show`がない話。簡単に書けるから自分で書けばいいじゃんという若干乱暴な結論。

#### `@ViewChild`と`@ContentChild`の違い
コンポーネント自身のテンプレートの中にあるのがViewで、外部から与えられるのがContent。
Angular 1のTranscludeとContentが同じものだという説明をしました。
これに関しては後日Qiitaあたりに記事を書きます。

#### Parse.comなき今、Angular2でさくっと遊びたいときのおすすめサーバー構成
Firebaseをおすすめしました。[AngularFire2](https://github.com/angular/angularfire2)もあるので、
さくっと作れそうだしFirebaseにデプロイもできてHerokuみたいに使えるよという話をしました

## 主催としてのまとめ
みなさん楽しんでもらえたようで何よりでした。第1回からすごく濃い内容だったので次回以降もとても楽しみです。

ng-sakeはミートアップに属するイベントで勉強会ではないので、発表中に聴衆から声があがったり突発的に議論が始まったり、なかなか自由な雰囲気でした。
これが全員に受け入れられるかはわかりませんが、堅苦しくないイベントにしたいと思っているので今後もこの雰囲気でやりたいと思います。
次回、4月の ng-sake をお楽しみに！

あと余談ですが、ディスカッション中にZoneが難解という話になったので「Zone.jsを紐解く会」も近々開催しようと思います。



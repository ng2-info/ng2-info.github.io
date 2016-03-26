title: Beta.12リリースと最近の動き
date: 2016-03-26 08:15:49
author: laco0416
tags:
- Beta
- Release
- Event
- Meeting
- Router

---

こんにちは、らこです。最近ng-japanやミートアップや普段の仕事などで忙しくて更新が滞ってました。

## ng-japan
ng-japanお疲れ様でした！現地にいた方もYoutubeで見ていた方も楽しんでもらえたでしょうか？
私は「[Angular 2の間違えない始め方](http://laco0416.github.io/slides/a-way-for-happy-angular-days/#/)」というセッションでトップバッターを務めさせてもらいまして、Twitterなんかの反応を見る限り好評だったようで嬉しいです。

全体のまとめは[ng-japan 2016 セッション資料まとめ - Qiita](http://qiita.com/nyamogera/items/b83833d1e15a55d0bb66)が参考になるでしょう。

## Beta.12リリース
Angular 2のBeta.12がリリースされました。予定外のリリースでバグがたくさん残っていたBeta.11の修正がメインですので、
Beta.11で入った変更も含めて紹介します

[angular/CHANGELOG.md at master · angular/angular](https://github.com/angular/angular/blob/master/CHANGELOG.md)

### `@View`アノテーションの廃止
前にも書きましたがついに廃止されました。今後は`@Component`にビューの情報も含めることが必須になります。

そもそもなぜ`@View`と`@Component`が共存していたかというと、
`@View`は1つのコンポーネントに複数付けられることが想定されていたからです。
同じコンポーネントでも状態によってビューのテンプレートを切り替えたい、
同じロジックでもセレクタによってビューを変えたいなど、動的なビューの切り替えを想定したものでした。
ただし現状ではそれを実装することはコストが重く、需要もほとんどないので、一旦すべて白紙に戻して仕様から練り直すことになりました。

### Zone.js 0.6.x
すぐにBeta.11が出てしまったのでほとんど気づかれていませんでしたが、Beta.10からZone.jsが0.6系になっています。
Zone.jsは0.5系までは単なるAngularのサブプロジェクトでしたが、0.6系からはES.next ProposalとなったZones仕様のpolyfillとして存在します。

Beta.11ではこの新しいZoneへの依存部分にバグがあったので、Beta.12で修正されました。
Beta.12の修正により、今まで必須だったZone.jsのlong-stack-traceがオプショナルになったので、commonjsでインポートする際は次の1行だけで実行可能になりました。

```
import "zone.js/dist/zone";
```

ただこれだとZoneをまたいだエラーのスタックトレースが追えなくなるので、特別な事情がなければlong-stack-traceも読み込んでおくほうが良いでしょう。

```
import "zone.js/dist/zone";
import "zone.js/dist/long-stack-trace-zone";
```

Zonesの仕様

[domenic/zones: Zones proposal for JavaScript](https://github.com/domenic/zones)

### peerDependenciesからes6-promiseを削除
peerDependenciesからes6-promiseが削除されました。
Zone.js 0.6系がPromiseのpolyfillを兼ねるようになったらしいので、ブラウザの対応状況に変化はありません。


## 今後のイベント情報
最近はAngular 2の機運もあって日本でたくさんAngular 2のイベントが開催されています。
募集開始直後に埋まってしまうものがほとんどなので、まだ参加できるものを紹介します

### [AngularJSハンズオン（初心者向け） in 広島 - Crunch-Lab](https://22525f535689619dc83bdcae89.doorkeeper.jp/events/41718)
広島で開催されるAngularJSのハンズオンです

### [ng-conf extended Tokyo](http://connpass.com/event/29136/)
私が主催するng-confのライブビューイングイベントです。これは抽選制にするので先着で埋まることはないです。

## その他
現在はRCに向けた作業がメインでコアチームは多忙なようですが、2,3週間後からはPRの整理を再開できるらしいです。
送ったパッチがなかなか取り込まれない期間でコントリビューションするにはやきもきしますが、今は待ちましょう。


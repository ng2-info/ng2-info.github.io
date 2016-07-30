+++
title = "1/11 週次ミーティング議事録"
date = "2016-01-12T11:12:16+09:00"
tags = ["Meeting", "Planning"]
+++

今週の週次ミーティングの議事録が公開されました。今週は2016年の方針決定を含めた重要な回でした。
先週の内容と重なる部分も多いですが、議事録と補足を。

[Angular Weekly Meeting](https://docs.google.com/document/d/150lerb1LmNLuau_a_EznPV1I1UHMTbEl61t4hZ7ZpS0/edit)

<!--more-->

---

### Tim presented "How Angular 2 transformer for Dart works"
去年から予告されているのですが、Angular2 for Dartにおいてtransformer（Dartのビルド時プリプロセッサ）がどう動いているのかを解説する記事が投稿されるようです。

### Moved Angular 2 to MIT license based on developer feedback/requests
Angular2のオープンソースライセンスがMITになりました。
主な理由は他の多くのライブラリと同じであり、わかりやすいからというリクエストが多かったからのようです。
詳細は[公式ブログ](http://angularjs.blogspot.jp/2016/01/angular-2-mit-open-source-licensed.html)で説明されています。

### Grand plan for 2016
2016年のプロジェクトの大まかなプランが発表されました。

#### Launch ng2 final (material design components, more perf, less payload, docs, animations, etc.)
**2016年中にAngular2を完成させます。** これはangular2-materialやドキュメント整備なども含めた完成です。

#### Mobile
モバイル環境では、"Progressive Web Apps"との連携にやる気充分です。
Progressive Web Appsは具体的にはServiceWorkerやPush Notification、Manifestなどを用いた
モバイル端末との連携機能によってネイティブアプリ同様のユーザ体験をもたらそうというものです。

[Progressive Web Appsについて - 銀色うつ時間](http://sisidovski.hatenablog.com/entry/2015/12/04/120633)

同じGoogle製のライブラリであるPolymerもServiceWorker連携のPolymer Elementsを公開していたので、
Angular2でもコアではなくオプションのライブラリによってServiceWorkerを使いやすくなる機能が追加されるかもしれません。

#### Desktop install infrastructure (via electron.js)
デスクトップ環境では、Node上でAngular2を動作させ、Electronとの連携を推し進めます。
ワーカーを使わなくてもブラウザ側で処理することもできますが、
分離しておくことで同じアーキテクチャがサーバサイドレンダリングでも、WebWorkerでも適用できます。

実際にAngular2のレンダリングをElectronのワーカーで行い、IPCでブラウザ側と通信するサンプルをrobwormald氏が作っています。
[robwormald/ng2-electron](https://github.com/robwormald/ng2-electron)

また、このワークフローをAngular CLIでサポートする予定があるようです。

#### Long term performance plans
次はパフォーマンス改善です。Angular2は最初からパフォーマンスを売りにしてきましたので、可能な限り向上させようという意気込みが感じられます。
実行速度のパフォーマンスは十分に速くなっているので、これからはペイロードサイズの削減などプロダクションを見据えた広い意味のパフォーマンスを改善していきます。

#### Reboot the web build system
これは初耳なのですが、現行のビルドシステムに変わる新しい物を作ろうとしているのでしょうか。
関連するドキュメントもなく詳細は不明です。

### Update on Angular 2 vs Angular 1 traffic
現状のAngular2のAngular1(AngularJS)のウェブサイトにおけるトラフィック比較が更新されました。

30日間のアクセス数は、AngularJS(angularjs.org)が1.06M、Angular2(angular.io)が210Kで、
Angular2が占める割合は19.8%まで上がりました（前回から10%増）。

### Mary Poppins Chatbot status update
SlackとGitHub上でタスクを自動処理するbot（Mary Poppins）を改修中です。

### Material Design for Angular 2
angular-materialのAngular2版(angular2-material)が始動し、現在はコアメンバーによるデザインの議論中です。
angular2-materialではバージョンごとの差異をスクリーンショットで比較できるツールを用意するようです。

---

Beta.1のリリース、グランドプランの決定など、年明け後も勢いは衰えてないですね。
3月にはng-japan、5月にはng-confが控えていますので楽しみにしていましょう。

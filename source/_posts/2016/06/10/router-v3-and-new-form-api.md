title: Router v3と新しいForm APIについて
date: 2016-06-10 19:09:12
author: laco0416
tags:
- Router
- BreakingChange
- Meeting
---

どうも、らこです。しばらく更新が止まってましたが、RC.2が近づいてきてそろそろ復活すると思います。

6/6のミーティングノートで、RC.2のリリース計画と、新しいRouterのリリースについて書かれました。

https://docs.google.com/document/d/150lerb1LmNLuau_a_EznPV1I1UHMTbEl61t4hZ7ZpS0/edit#heading=h.p095qktniiv3

どれも寝耳に水ではありますが、ここしばらく更新が緩やかだったのはこのために下準備をしていたということがわかってスッキリです。
ここからはミーティングノートの内容をひとつずつ見ていきます。

---

## RC.2のリリースについて
RC.2のゴールは、Offline Template Compilerを使ってのコンパイルを可能にすることです。
ほとんど完成していますが、まだいくつかのissueが残っているので、それらが解決したところでRC.2のリリースとなります。

## Form APIの刷新
いままで特に変更がなかった `ngForm` や `ngModel`, `FormBuilder` など、 `@angular/common` Form APIに、大きな変更が入ります。
担当はAngular Material2と同じKara Erickson氏で、 鋭意作業中です。
まだ新しいAPIは完成していませんが、デザインドキュメントは公開されているので、どう変わるのかを先に知っておくことはできます

[Forms Upcoming Change Proposal](https://docs.google.com/document/d/1RIezQqE4aEhBRmArIAS1mRIZtWFf6JxN_7B4meyWK0Y/pub)

基本的な方針は、Angular 1のようなテンプレートドリブンのAPIにすることです。
今までは `ngModel` を書いていても `ngControl` も併記しないといけなかったり、 `ngForm` や `ngFormControl` など混乱しがちなAPIが多くあったことが問題とされていて、
テンプレートドリブンで書きやすいような設計に書きなおされます。逆に、モデルドリブンなFormの組み立てはオプショナルな機能になってしまいます。

## Router v3  
Angular2公式のRouterが新しくリリースされ、ver3.0.0-alpha.3となりました。

リポジトリはこちら

[angular/vladivostok](https://github.com/angular/vladivostok)

メインで開発しているのはお馴染みVictor Savkin氏です。
リポジトリの名前はウラジオストクですが、深い意味はありません。おそらくvictorsavkinから取られてるんじゃないかという気がします。

[Question: why vladivostok? · Issue #31 · angular/vladivostok](https://github.com/angular/vladivostok/issues/31)

>we ran out of names for routers.

Beta Router( `@angular/router-deprecated` )をv1として、
RC Router( `@angular/router@2.0.0-rc.1` )がv2になるのですが、そしてそれらを過去にするようやくまともなルーターが出てきました。
v1, v2からの反省を活かしつつ、 `@ngrx/router` や `ui-router` の思想を取り入れたv3を今後は使うことになります。

v2までの問題点と、v3を作るに至った経緯、基本思想は公式ブログで綴られています

[Improvements Coming for Routing in Angular](http://angularjs.blogspot.jp/2016/06/improvements-coming-for-routing-in.html)

Router v3の具体的な使い方はSavkin氏のブログにかかれています。私も触ってみたので[こちら](http://blog.lacolaco.net/post/angular-2-router-v3-alpha-3/)で解説しています。

[Angular Router | Victor Savkin](http://victorsavkin.com/post/145672529346/angular-router)

公式のドキュメントも2週間以内には公開するとのことです。


---

ng-confとGoogle I/Oが終わってから、Angular Teamはイベントから開放されてひたすらRCを成熟させるためにコードを書いてくれているようです。
Pull Requestも以前とは比較にならないほどスムーズにやりとりが進んで、マージされるハードルも低くなっているので、
RCを触っていて見つけたバグは積極的に報告し、PRを送ってみるといいんじゃないかと思います。

次はRC.2がリリースされたら変更点を紹介する予定です。それでは。



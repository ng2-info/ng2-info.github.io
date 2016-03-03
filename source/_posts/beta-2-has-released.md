title: Angular2 Beta.2のリリース
date: 2016-01-30 01:25:58
author: laco0416
tags:
- Beta
- Release
---

Angular2のBeta.2バージョンがリリースされました。

[CHANGELOG](https://github.com/angular/angular/blob/master/CHANGELOG.md)


そこそこに大きいリリースですが、大半はバグ修正なのでアップデートをためらう理由はないでしょう。
変更点の中で気になるものについて取り上げます。

### core/application_ref: Allow asyncronous app initializers. (df3074f), closes #5929 #6063
[df3074f](https://github.com/angular/angular/commit/df3074f)

Angular2の起動時に解決されるDIのひとつに`APP_INITIALIZER`というプロバイダがあります。
`APP_INITIALIZER`はデフォルトでは何も設定されていませんが、関数が注入された場合は
アプリケーションの起動前に実行されるものです。

これまではただ実行するだけでしたが、戻り値がPromiseだった場合は完了を待ってからアプリケーションを起動するようになりました。

### test: allow tests to specify the platform and application providers used (b0cebdb), closes #5351 #5585 #5975
[b0cebdb](https://github.com/angular/angular/commit/b0cebdb)

testingパッケージがプラットフォームごとにProviderを切り替えられるようになりました。
現在はbrowserとbrowser_static、serverの3つがありますが、具体的な手法についてはまだbrowserでのテストしかドキュメントがありません。
serverプラットフォームについてはもうしばらくドキュメントを待ちましょう。

### build(node): split test and src compilation units
[a4b5cb8](https://github.com/angular/angular/commit/a4b5cb837682ce61f8c07eabd7ae8c4dc3ba80a7)

これはCHANGELOGには含まれていないのですが個人的にはBeta.2最大の変更です。
Angular2本体の型定義ファイルから、Angular2自体のテストに必要な依存ライブラリを排除しました。

具体的にはJasmineとProtoractorの型定義を参照しないようにしたので、
以前からの問題であったjQueryの`$`変数とProtoractorの型定義が衝突する問題や、
Jasmineとその他のテスティングフレームワーク（mochaなど）で型定義が衝突する問題が解消されました。


beta.3は来週リリースされる予定です。週次になるとそこまで大きなアップデートにはならないと思われますが、
Router周りの改善が予告されているので楽しみにしていましょう。

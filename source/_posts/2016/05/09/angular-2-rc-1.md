title: Angular 2 RCリリースについて
date: 2016-05-09 11:35:30
author: laco0416
tags:
- Release
- RC
- BreakingChange

---

ng-confお疲れ様でした、らこです。
Angular 2もついにRelease Candidateとなりまして、最終リリースへのステップをまたひとつ進めたところです。
Betaから大きく変わっている部分もありますが基本的には機械的に対応可能な破壊的変更なので、さくっとRC対応していきましょう。

---

## パッケージの変更
RCからはnpmで配布するパッケージ自体が変わりました。
今まではすべて `angular2` パッケージの中にまとめられて配布されていましたが、今後は細かい単位でパッケージが分割されます。
古い`angular2`のモジュールと新しいパッケージの対応は以下のようになっています。

| `angular2/***` | `@angular/***` |
| :-----: | :--------: |
| core | core |
| common | common |
| compiler | compiler |
| testing | core/testing, compiler/testing, platform-browser/testing,... |
| platform/browser|  platform-browser, **platform-browser-dynamic** |
| platform/server | platform-server |
| platform/common | **common** |
| http | http| 
| router | **router-deprecated** | 
| alt_router | **router** | 
| upgrade | upgrade | 

`@angular/core` というパッケージ名は、npmの _scoped package_ という形式で、
「"angular" organizationの "core" パッケージ」という意味になります。
具体的には次のようなインストールコマンドを使います。

```
$ npm i @angular/core @angular/compiler @angular/common @angular/platform-browser @angular/platform-browser-dynamic
```

パッケージは小分けになりましたが、各パッケージはpeer dependencyで依存しあっているので、
もし足りないパッケージがあったとしてもインストール後にwarningが発生して教えてくれます。

ところで、一部のAPI、`AnimationBuilder`や`BrowserDomAdapter`などがパブリックなAPIから消滅しています。
内部的には存在していて、無理やり使用することはできますが、自己責任で。
おそらくRC以降に巻き込まれて一時的に隠れているだけだと思われます。

## 依存パッケージの変更
今までAngular 2はいくつかのpeer dependencyを持っていましたが、それが最小限のものだけになりました。
具体的には、`es6-shim`と`reflect-metadata`がpeer dependencyから削除されました。
これは依存しなくなったわけではなく、使用するpolyfillをユーザーに委ねるようにした、いわゆるポリシー変更です。
ES6機能のpolyfillには以前からcore-jsなど他のものを使うことができましたが、es6-shimをインストールしないとwarningがでるという状態だったので、
扱いやすい形に変わったといえます。

## platform-browserとplatform-browser-dynamicの分割
従来通りランタイムでbootstrapする方式と、Offline Compilerを使ったstaticなbootstrap方式の両方をサポートするため、
ブラウザプラットフォーム用のパッケージが`@angular/platform-browser`と`@angular/platform-browser-dynamic`に分割されました。
元々`angular2/platform/browser`からexportされてた`bootstrap`関数は、`@angular/platform-browser-dynamic`に含まれています。

## alt_routerがrouterに昇格
今までの`angular2/router`モジュールは`@angular/router-deprecated`パッケージとなり、
その名の通り過去のものになってしまいました。
そしてVictor Savkinが作りなおした新しいパッケージが`@angular/router`として配布されています。

しかしまだ`@angular/router`は開発途上なので、旧routerから移行するには機能が足りていない場合があります。
既存のAngular 2アプリケーションはまだしばらく`@angular/router-deprecated`でもよさそうです。

---

Beta.17まで順調についてきていればあとはパッケージ名の置換だけで済むはずなので、さくっとRC対応できるはずです。
RCも短いスパンでアップデートされていくのが目に見えているので、置いていかれないようにしていきましょう。

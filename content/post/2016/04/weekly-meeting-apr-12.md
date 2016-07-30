+++
title= "週次ミーティング"
date= "2016-04-12T21:24:38+09:00"
tags =["Meeting","TypeScript","Mobile","Planning"]

+++

どうも、らこです。今週も週次ミーティングの内容を噛み砕いて紹介します。

<!--more-->

---

## 4/11 週次ミーティング

### コアライブラリの変更
Angular 2のコア部分に2つ大きな変更があります。

1つ目は、テンプレートのコンパイルやDIの解決、Change Detectionなどのコア機能を静的コード生成に委ねるようにする変更です。
この変更によりパフォーマンスが格段に改善されるとともに、アプリケーションのデプロイ時のサイズを削減することができます。

2つ目はbootstrapの方法が変わり、ツリーシェイキングやデッドコードの削除が可能になります。

### Angular Material 2
Angular Material 2のAlpha.2がリリースされました。
今後のリリース予定についてはGitHubのマイルストーンで確認できます。

[Milestones - angular/material2](https://github.com/angular/material2/milestones)

### ドキュメンテーション
今後もドキュメンテーションの拡充を進めていきます。「Why Angular?」というコンテンツが追加される予定で、
ドキュメンテーションのランディングページも改善されます。

また、モバイル向けの情報やプラットフォームなどにより細かくサブドメインを分けます。

angular.ioには新しく次のドキュメントが追加されています。

- [Component Interaction - ts](https://angular.io/docs/ts/latest/cookbook/component-communication.html)
- [Dependency Injection - ts](https://angular.io/docs/ts/latest/cookbook/dependency-injection.html)
- [Dynamic Form - ts](https://angular.io/docs/ts/latest/cookbook/dynamic-form.html)
- [Component Styles - ts](https://angular.io/docs/ts/latest/guide/component-styles.html)

### 周辺ツール

### スタイルガイド
John Papaがスタイルガイドの開発をMinko Gechevらと進めています。

- [mgechev/angular2-style-guide: Community-driven set of best practices and style guidelines for Angular 2 application development](https://github.com/mgechev/angular2-style-guide)

#### Angular CLI
スタイルガイドをもとにコードのブループリントを実装しています。
その次にはオフラインテンプレートコンパイルとの連携を実装します。

- [angular/angular-cli: CLI tool for Angular2](https://github.com/angular/angular-cli)

#### Batarangle
Batarangleはもうすぐ正式リリースされます。
コンポーネントやDI、ルートの可視化ができます。 

- [rangle/augury: Angular 2 development tools for Chrome](https://github.com/rangle/augury)

#### Language Services
TypeScriptのLanguage Servicesで、Angular 2のテンプレート内の入力補完を可能にする計画です。
そのために必要なコンポーネントメタデータの抽出はすでにtboschが実装を終えており、
次はそのデータをエディターで使えるようにする段階です。

- [Tracking: Language services provide intellisense for Angular 2 templates · Issue #7482 · angular/angular](https://github.com/angular/angular/issues/7482)
- [TypeScript extensibility · Issue #6508 · Microsoft/TypeScript](https://github.com/microsoft/typescript/issues/6508)

#### ngLint
Minko GechevがAngular 2のlinterを開発しています。
Language Serviceと統合することでコマンドラインやIDE上にメッセージを表示できるようになります。

- [mgechev/codelyzer](https://github.com/mgechev/codelyzer)

### Router
Finalリリースに向けて幾つかのAPIの変更が行われます。
将来的に、コードの分割と遅延読み込みが自動的に行えるようになります。
APIを今よりシンプルにして混乱を避けるようにし、クリティカルなバグを取り除きます。

### Animation
まもなくngAnimate2の最初のリリースが行われる予定です。

- [wip: feat(animations): introduce animation support for angular2 by matsko · Pull Request #7698 · angular/angular](https://github.com/angular/angular/pull/7698)

### i18n
文字列抽出とngPluralディレクティブが実装されています。
残りは実装した機能を実際に使いやすくするステップです。
ただし残りの作業は最初のコアの変更にブロックされています。

### TypeScript @ Google
GoogleもTypeScriptの開発に協力しているようで、そのチームによる最初の成果がプロダクションに入るようです。

### モバイル
Angular 2を使ってProgressive Web Appを作りやすくするようにするのが目的の取り組みです。
まずはGitHub管理アプリをサンプルとして開発中です。
計画ではその成果をng-confとGoogle I/Oで紹介する予定です。

これらについてはmobile.angular.ioで別サイトとして独立したコンテンツになります。

### Marcy Sutton
Marcy SuttonさんがMountain Viewに来るのでアクセシビリティについてAngularチームとミートアップするようです。
また、アクセシビリティについてのクックブックを作成中です。

### Angular CLI
Hansがデザインドキュメントを作成中です。
次のバージョンではng1からのマイグレーションにも対応するようです。

---

てんこもりの内容でした。ng-confに向けてどんどん開発が加速しています。今週のbeta.15もてんこもりの内容になりそうなので、要チェックです。  


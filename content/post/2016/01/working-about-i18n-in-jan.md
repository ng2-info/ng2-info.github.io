+++
title = "i18n対応の現状について"
date = "2016-01-16T15:45:25+09:00"
tags= ["Planning"]
+++

今回はAngular2の国際化対応(i18n)について。

<!--more-->

AngularJSではi18nを行うためにangular-translateなどのサードパーティ製ライブラリを使う必要がありました。
ところがAngular2ではライブラリの機能として、公式でi18nをサポートすると宣言しています。

[INTERNATIONALIZATION (I18N) & ACCESSIBILITY](https://angular.io/features.html)

>Reach all your users. Use the familiar ICU message format in Angular interpolation syntax ({% raw %}{{ }}{% endraw %}), including pluralization and gender rules.
Automate message extraction, pseudo-localization, and translation updates.
Generate static applications for each locale.
Easily promote accessibility via screen readers and assistive devices by automatically generating appropriate ARIA attributes.

この説明文によれば`{% raw %}{{}}{% endraw %}`で展開されるデータバインディングは自動で適切にローカライズされると書かれています。

しかし今のところこの機能はほぼ進展がなく、i18n用と思われるリポジトリ [angular/i18n](https://github.com/angular/i18n) は
開発途上のまま止まっています。
Angular2もbetaになったことでユーザからi18n機能の進展についての質問が増え始めましたが、
Issueでのやり取りで現状が把握できました。

[i18n and L10n support · Issue #5953 · angular/angular](https://github.com/angular/angular/issues/5953)

今のところ、公式のi18nはコアメンバーの多忙により進展がないようですが、
ocombe氏による[ng2-translate](https://github.com/ocombe/ng2-translate/)が開発中のようです。
ng2-translateはangular-translateをAngular2に移植するプロジェクトで、
まだすべての機能は移行できていませんが一部はすでに使用可能とのこと。

また、ocombe氏は公式のi18n機能が実装されたとしてもそれが自身の要求を満たさないものであれば、
ng2-translateの開発を継続するつもりらしいので、
今はとりあえずng2-translateを使っておけばどちらに転んでも大丈夫そうです。
（開発している本人が自分のためにマイグレーションをサポートするでしょう）

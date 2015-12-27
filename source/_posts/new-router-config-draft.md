title: Routerモジュールの刷新が計画中
date: 2015-12-27 18:31:48
tags: 
- Router 
- Draft
---

beta.0バージョンがリリースされて比較的安定したAngular2ですが、
現在水面下でRouterモジュールを刷新する計画が進行中です。

## 現在のRouterモジュール

現在のRouterモジュールでルーティング設定するには、ルーティングを行うコンポーネントに対して`@RouterConfig`デコレータを用います。
RouterConfigはルートの設定を表現するRouteDefinitionインタフェースのオブジェクトを受け取ります。

```
@RouteConfig([
  {path: '/', redirectTo:['Home'] }
  {path: '/home', component: HomeComponent, name: 'Home' }
  {path: '/foo', component: FooComponent, name: 'Foo' }
])
@Component({
  selector: 'my-app',
  template: `
  <a [routerLink]="['Home']">Home</a>
  <a [routerLink]="['Foo']">Foo</a>
  `
})
class MyApp {}
```

RouteConfigは必ずコンポーネントのクラスに対して設定するため、ルーティングとビューが密結合になってしまいます。

また、現在のRouterモジュールの設計は`redirectTo`や、
ルーティングを行う際に用いる`routerLink`など、配列を用いている部分があります。
これはデータを付与してルーティングする際に用いられるもので、例えば`/team/1/user/2`というようなルーティングを行う場合は次のような表現を使うことができます。

```
[routerLink]="['/Team', {teamId: 1}, 'User', {userId: 2}]"
```

これらの文字列を使った表現はもはやDSLになりつつあり、開発者の学習コストが高くなっています。
そのため、Routerモジュールを再設計する動きが開発チーム内で始まっています。

## Class-based Router Config

先日、GitHubのIssueへの[コメント](https://github.com/angular/angular/issues/6040#issuecomment-166456198)に
新しいRouterモジュールについてのドキュメントが貼られました。

[Class-based Router Config: Google Docs](https://docs.google.com/document/d/1IKZLXU9Y3zdnedj5M7LfW5HQEDf9zyVjNpk_79Rf3SQ/edit?ts=56611fd1)

※ このドキュメントの内容はまだドラフト段階で不定期に更新されるので、この記事の内容と食い違う部分がある可能性があります。

新しいRouterConfigはコンポーネントに対するデコレーションではなく、独立したクラスとして定義します。
`@RouterConfig`デコレータをつけたクラスのプロパティとして個別のRouteを定義します。
`@Route`デコレータには紐付けるコンポーネントのクラスを渡し、
プロパティは返り値としてそのRouteのパス表現を文字列で返す関数を定義します。
パス中にパラメータを取りたい場合は関数の引数として受け取って文字列に含めます。

```
@RouterConfig()
class UserRoutes {

  @Route({
    component: UsersComponent,
  })
  users = () => `users`; // returns url path for this route to be prefix to base url


  @Route({
    component: UserComponent,
    canActivate: (somethingInjectable: SomethingInjectable) => somethingInjectable.areWeGood(),
  })
  user = (userId: number) => `user/${userId}`;

}
```

`@RouterConfig`がつけられたクラスはルーティングを行いたいコンポーネント側でDIによってインスタンスを受け取ります。
インスタンスのメソッドを呼び出すことでパスが生成され、それを`[routerLink]`に渡すことができるようになります。
配列を用いた不自然なDSLではなく他のデータバインディングと同じ`.`つなぎのメソッド呼び出しなのでとてもわかりやすくなりました。

```
@Component({
  selector: 'my-user',
  template: `
    <a [routerLink]="userRoutes.user(3)">Go to user with Id=3</a>
    <button (click)="usersButtonClicked()">back to all users</button>
  `
})
class UserComponent {
  constructor(private userRoutes: UserRoutes, private router: Router) {}

  usersButtonClicked() {
    this.router.navigate(this.userRoutes.users());
 }
}
```

この再設計はbetaリリース前に行いたかったようですが間に合わなかったとのことで、おそらくbetaの早い時期に導入されるでしょう。
現在のRouterとはまるで互換性がないのでマイグレーションなども含めて課題はありますが、
良くなる方向の破壊的変更はどしどし入れて欲しいものです。まだbetaですからね。

## What's new in Angular 9?
https://blog.angular.io/version-9-of-angular-now-available-project-ivy-has-arrived-23c97b63cfa3
https://blog.angular.io/version-9-1-of-angular-now-available-typescript-3-8-faster-builds-and-more-eb292f989428
https://www.one-tab.com/page/8kTLJLCkSZSG6Qs9YzSJyQ

#### Ivy Renderer
Now by default in new projects, quick setting in tsconfig for existing ones.

4th renderer, IV -> Ivy, just a quick lil fun fact.

<What is Ivy in 3 lines, show quick code example with differences, benefits>

From Angular blog:
Smaller bundle sizes
Faster testing
Better debugging
Improved CSS class and style binding
Improved type checking
Improved build errors
Improved build times, enabling AOT on by default
Improved Internationalization


#### Angular Core typesafety improvements
TestBed.get(SYMBOL) -> TestBed.inject<SYMBOL>(Symbol)

#### AoT compiler by default
ngcc - Angular Compatibility Compiler, to allow backwards compatibility with older view engine API's

#### Improved i18n (Internationalization)
https://angular.io/guide/i18n
- Directionality Query API



#### Improved testing
- Component harnesses
Zie angular blog, is wel interessant!

- End-to-End tests now support grep and invertGrep

#### DevEx: IDE & Language Service improvements
Mainly for VSCode
Improved HTML & Expression Syntax Highlighting


#### TypeScript upgrades
TS 3.7 in v9, 3.8 in v9.1
Adds: Optional chaining, Type-only imports, ECMAScript private fields, top level await, "fast & loose" incremental checking.
https://devblogs.microsoft.com/typescript/announcing-typescript-3-8/

#### TSLint 6.1 by default (v9.1)
No automatic upgrade, cause of breaking changes.

`ng update @angular/cli --migrate-only tslint-version-6`
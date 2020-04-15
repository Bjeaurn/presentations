---
title: What's new in Angular v9?
notesSeparator: "^Note:"
---

# What's new in Angular v9?

// TODO - Picture of yourself and Codestar on this one? Github link etc.? Twitch reference?

Note: Introduce yourself

---

## Angular moves fast
- A major release every 6 months
- 1-3 minor releases for each major release
- A patch release and pre-release (next or rc) build almost every week

https://angular.io/guide/releases

---

# So what is new in Angular 9?

---

# <span class="fragment highlight-red" data-fragment-index="1">IV</span>Y Renderer

<p class="fragment fade-in" data-fragment-index="0">Now default!</p>

<aside class="notes">
1) This is however, not an Ivy talk. We'll glance over it and what it brings, but for more in-depth detail I'll refer you to some other excellent talks.

2) 4th renderer, where the name "IV" came from.
</aside>

----

<p class="fragment fade-in-then-semi-out visible" data-fragment-index="0">Smaller bundles</p>
<p class="fragment fade-in-then-semi-out visible" data-fragment-index="1">Better debugging</p>
<p class="fragment fade-in-then-semi-out visible" data-fragment-index="2">Faster testing</p>
<span class="fragment" data-fragment-index="3"></span>

// TODO Add code examples per piece?

<!-- From Angular blog:
Smaller bundle sizes
Faster testing
Better debugging
Improved CSS class and style binding
Improved type checking
Improved build errors
Improved build times, enabling AOT on by default
Improved Internationalization -->

----

Now by default in new projects, quick setting in tsconfig for existing ones.
```json
  ...,
  "angularCompilerOptions": {
    "enableIvy": true
  },
  ...
```

---

# Improved i18n

Internationalization

Introduction of `@angular/localize`


https://angular.io/guide/i18n
- @angular/localize is new

`ng add @angular/localize`
https://blog.ninja-squad.com/2019/12/10/angular-localize/

Still have different bundles for every language. But compilation and building each bundle is now done in seconds or even parallel, taking international builds from 2 minutes to 40 seconds.


/// TODO !
```https://blog.ninja-squad.com/2019/12/10/angular-localize/
Runtime translations
As I was mentioning, if you use the CLI commands above (ng serve --configuration=fr or ng build --localize) then the application is compiled and then translated before hitting the browser, so there are no $localize calls at runtime.

But $localize has been designed to offer another possibility: runtime translations. What does it mean? Well, we would be able to ship only one application, containing $localize calls, and before the application starts, we could load the translations we want. No more N builds and N bundles for N locales \o/

Without diving too much into the details, this is already possible with v9, by using the loadTranslations function offered by @angular/localize. But this has to be done before the application starts.

You can load your translations in polyfills.ts with:

import '@angular/localize/init';
import { loadTranslations } from '@angular/localize';

loadTranslations({
  '1815172606781074132': 'Bonjour {$name}\xa0! Vous avez {$userCount} utilisateurs.'
});
As you can see there is no locale consideration: you just load your translation as an object, whose keys are the strings to translate, and the values, their translations.

Now if you run a simple ng serve, the title is displayed in French! And no more need for ng xi18n, or messages.fr.xlf or specific configuration for each locale in angular.json. In the long term, when this will be properly supported and documented, we should be able to load JSON files at runtime, like most i18n libraries do. You could even achieve it in v9, it’s just a bit of manual work, but it’s doable.
```

- Directionality Query API

---

# Angular Core typesafety improvements

```
TestBed.get(SYMBOL)
TestBed.inject<SYMBOL>(Symbol)
```

Unfortunately at the time there's no auto migration for this. So a bit of manual labor in your unit tests.

---

# Testing harnasses

- Component harnesses
- End-to-End tests now support grep and invertGrep

---

# AoT compiler by default
The Ahead of Time compilation was an option already, which made some more dynamically generated things harder to do and caused issues in earlier versions. This is now all fixed. 

The `ngcc`, (Angular Compatibility Compiler) enables that older versions of libraries are compatible with the current version Ivy Renderer and AoT.

---

# TypeScript & TSLint upgrades

-TS 3.7 in v9, 3.8 in v9.1
- Adds: Optional chaining, Type-only imports, ECMAScript private fields, top level await, "fast & loose" incremental checking.
https://devblogs.microsoft.com/typescript/announcing-typescript-3-8/

- TSLint 6.1 by default (v9.1)
- No automatic upgrade, cause of breaking changes.

`ng update @angular/cli --migrate-only tslint-version-6`

---

# DevEx: IDE & Language Service improvements
- Mainly for VSCode
- Improved HTML & Expression Syntax Highlighting

---

That's all folks!
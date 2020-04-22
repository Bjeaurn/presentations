---
title: What's new in Angular v9?
notesSeparator: "^Note:"
---

## What's new in Angular v9?

<div style="float: left; width: 40%">
  <img src="assets/bjorn.jpg" width="100" style="border-radius:100%; display: inline-flex;"><br />
  <img src="assets/codestar.svg" height="30" style="border: 0; background-color: transparent;">
</div>
<div style="float: left; width: 40%; text-align: left;">
<br />
  <h1 style="font-size: 0.9em;">Bjorn Schijff</h1>
  <small style="display: inline-flex;">Frontend Software Engineer @ Politie</small><br />
   <small>@Bjeaurn / bjorn.schijff@ordina.nl</small>
</div>

Note: Introduce yourself

---

## Angular moves fast
- A major release every 6 months<!-- .element: class="fragment" -->
- 1-3 minor releases for each major release<!-- .element: class="fragment" -->
- A patch release and pre-release (next or rc) build almost every week<!-- .element: class="fragment" -->

https://angular.io/guide/releases<!-- .element: class="fragment" -->

---

# So what is new in Angular 9?

---

# <span class="fragment highlight-red" data-fragment-index="1">IV</span>Y Renderer

<p class="fragment fade-in" data-fragment-index="0">Now default!</p>

<aside class="notes">
1) This is however, not an Ivy talk. We'll glance over it and what it brings, but for more in-depth detail I'll refer you to some other excellent talks.

2) So, most important thing for V9: Ivy is DEFAULT!

3) 4th renderer, where the name "IV" came from.
</aside>

----

<p class="fragment fade-in-then-semi-out visible" data-fragment-index="0">Smaller bundles</p>
<p class="fragment fade-in-then-semi-out visible" data-fragment-index="1">Better re-compilation performance</p>
<p class="fragment fade-in-then-semi-out visible" data-fragment-index="2">Better debugging</p>
<p class="fragment fade-in-then-semi-out visible" data-fragment-index="3">Faster testing</p>
<span class="fragment" data-fragment-index="3"></span>

----

### Bundle sizes without Ivy

![No Ivy](./assets/sizes/noIvy.png)

```sh
main es2015: 75.7kb
main es5: 78kb
```

Note: Angular v9.1.1

----

### Bundle sizes with Ivy

![With Ivy](./assets/sizes/withIvy.png)

```sh
main es2015: 59.2kb
main es5: 61.9kb
```

----

### Better re-compilation

![No Ivy](./assets/recompilation/noIvy.png)

![With Ivy](./assets/recompilation/withIvy.png)


----

### Better debugging (without Ivy)

![No Ivy](./assets/debugging/noIvy.png)

----

### Better debugging with Ivy

![With Ivy](./assets/debugging/withIvy.png)

----

### Faster testing

![No Ivy](./assets/testing/noIvy.png)
![With Ivy](./assets/testing/withIvy.png)

Note: This doesn't seem much, but scales greatly over a larger project.

----

Now by default in new projects, quick setting in tsconfig for existing ones.
```json
  ...,
  "angularCompilerOptions": {
    "enableIvy": true
  },
  ...
```$$

---

# Improved i18n

@angular/localize<!-- .element: class="fragment" -->

`ng add @angular/localize`<!-- .element: class="fragment" -->

https://angular.io/guide/i18n<!-- .element: class="fragment" -->

----

```html
<h1 i18n="meaning|Description for translators@@customId">Hello all!</h1>
```

```sh
ng xi18n
```
<!-- .element: class="fragment" -->

----

```xml
<xliff version="1.2" xmlns="urn:oasis:names:tc:xliff:document:1.2">
  <file source-language="en-US" datatype="plaintext" original="ng2.template">
    <body>
      <trans-unit id="customId" datatype="html">
        <source>Hello all!</source>
        <context-group purpose="location">
          <context context-type="sourcefile">src/app/app.component.html</context>
          <context context-type="linenumber">1</context>
        </context-group>
        <note priority="1" from="description">Description for translators</note>
        <note priority="1" from="meaning">meaning</note>
      </trans-unit>
    </body>
  </file>
</xliff>
```

```xml
<target>Hallo allemaal!</target>
```
<!-- .element: class="fragment" -->

```sh
ng xi18n  --format=xlf
ng xi18n  --format=xlf2
ng xi18n  --format=xmb
```
<!-- .element: class="fragment" -->

Note: 3 formats, XLF, XLF2 and XMB. All "known" translation formats.

----

```html
<span i18n>Updated {minutes, plural, 
  =0 {just now} 
  =1 {one minute ago} 
  other {{{minutes}} minutes ago}}
</span>
```

```xml
<trans-unit id="5420212fa00d960d1c8858ce09b10e0dc5b87d90" datatype="html">
      <source>Updated <x id="ICU" equiv-text="{minutes, plural, =0 {...} =1 {...} other {...}}"/>
      </source>
```
<!-- .element: class="fragment" -->


```xml
<trans-unit id="5a134dee893586d02bffc9611056b9cadf9abfad" datatype="html">
        <source>{VAR_PLURAL, plural, =0 {just now} =1 {one minute ago} other {<x id="INTERPOLATION" equiv-text="{{minutes}}"/> minutes ago} }</source>
</trans-unit>
```
<!-- .element: class="fragment" -->

----

### i18n is getting powerful!

`ng build --prod --localize`

- Faster building of all bundles<!-- .element: class="fragment" -->
- Still a bundle per locale<!-- .element: class="fragment" -->

<aside class="notes">Still have different bundles for every language. But compilation and building each bundle is now done in seconds or even parallel, taking international builds from 2 minutes to 40 seconds.

Of course many plugins available that do live translations etc. like `ngx-translate`.
</aside>

---

### Angular Core typesafety improvements

```ts
const service = TestBed.get(ExampleService)
service. ??? // any!
```
<!-- .element: class="fragment" -->

```ts
const service = TestBed.inject<ExampleService>(ExampleService)
```
<!-- .element: class="fragment" -->

No auto migration yet!<!-- .element: class="fragment" -->

---

### Testing

// TODO HIERZO! 

----

- Component harnesses
https://medium.com/@kevinkreuzer/test-your-components-using-angular-materials-component-harnesses-f9c1deebdf5d

----

- End-to-End tests now support grep and invertGrep
```ts
example!
```

---

### AoT compiler by default
The Ahead of Time compilation was an option already, which made some more dynamically generated things harder to do and caused issues in earlier versions. This is now all fixed. 

The `ngcc`, (Angular Compatibility Compiler) enables that older versions of libraries are compatible with the current version Ivy Renderer and AoT.

---

### TypeScript  upgrades


<p class="fragment fade-in-then-semi-out visible" data-fragment-index="0">v9: TypeScript 3.7</p>
<p class="fragment fade-in visible" data-fragment-index="1">v9.1: TypeScript 3.8</p>

----

<p class="fragment fade-in-then-semi-out visible" data-fragment-index="2">Optional chaining</p>
<p class="fragment fade-in-then-semi-out visible" data-fragment-index="3">Type-only imports</p>
<p class="fragment fade-in-then-semi-out visible" data-fragment-index="4">ECMAScript private fields</p>
<p class="fragment fade-in-then-semi-out visible" data-fragment-index="5">Top level await</p>
<p class="fragment fade-in-then-semi-out visible" data-fragment-index="6">"fast & loose" incremental checking</p>

https://devblogs.microsoft.com/typescript/announcing-typescript-3-8/<!-- .element: class="fragment" -->

---

### TSLint upgrade

- TSLint 6.1 by default (v9.1)<!-- .element: class="fragment" -->
- No automatic upgrade, cause of breaking changes.<!-- .element: class="fragment" -->

`ng update @angular/cli --migrate-only tslint-version-6`
<!-- .element: class="fragment" -->

---

### DevEx: IDE & Language Service improvements
- Mainly for VSCode
- Improved HTML & Expression Syntax Highlighting

---

## That's all folks!<!-- .element: class="fragment fade-in-then-semi-out"-->

# Questions?<!-- .element: class="fragment" -->

<div style="float: left; width: 40%">
  <img src="assets/bjorn.jpg" width="100" style="border-radius:100%; display: inline-flex;"><br />
  <img src="assets/codestar.svg" height="30" style="border: 0; background-color: transparent;">
</div>
<div style="float: left; width: 40%; text-align: left;">
<br />
  <h1 style="font-size: 0.9em;">Bjorn Schijff</h1>
   <small>@Bjeaurn / bjorn.schijff@ordina.nl</small>
</div>
---
highlightTheme: github-dark
---
<img src="assets/angular.svg" style="height: 15vh;" />

## What's new in Angular?

Note: Angular is changing, new things are being introduced all the time. Let's take a look at what's been happening and how it helps you in your daily job. You might not get the chance in your daily job to keep up and play with all the new things, so that's why I'm going to take you through it in the next 20-30 minutes.

----

### Angular update cycle

- Every 6 months<!-- .element: class="fragment" -->
- November 2023: v17<!-- .element: class="fragment" -->
- Current LTS: 14 and 15<!-- .element: class="fragment" -->
- Active: 16<!-- .element: class="fragment" -->

https://angular.io/guide/releases#support-window<!-- .element: class="fragment" -->

Note:  v14 (lts ends 18-11-'23, released 06-'22), 
v15 (lts ends 18-05-'24, release 11-'22), v16 (03-05'23, LTS at 03-11-'23). So if you're not on one of those, you should...

----


### Angular v16

- Signals (beta)<!-- .element: class="fragment" -->
- Standalone all the things<!-- .element: class="fragment" -->
- Non-destructive app hydration<!-- .element: class="fragment" -->
- Vite + ESBuild<!-- .element: class="fragment" -->
- Jest & Web Test Runner<!-- .element: class="fragment" -->
- Injectable DestroyRef<!-- .element: class="fragment" -->
- And much more!<!-- .element: class="fragment" -->

Note: The current active version is V16. In the next 20-30 minutes we'll go over all of these features and changes!

---

<div class="fragment fade-in">
  <img src="assets/bjorn.jpg" width="100" style="border-radius:100%; display: inline-flex;" class="border">
  <h1 style="font-size: 0.9em;">Bjorn Schijff</h1>
  <small style="display: inline-flex;">Sr. Frontend Engineer / Architect / Codesmith</small><br />
   <small><img src="assets/x.svg" style="width: 32px; padding-right: 12px; float: left;" /> <span style="float: right; padding-top: 1rem;">@Bjeaurn</span></small><br />
  <img src="assets/codestar.svg" height="30" class="clean">
  <img src="assets/ordina.svg" height="30">
</div>

Note: Introduce myself, and briefly describe that we're going to go over a tool that might help you keep this growth under control.

---

## Signals (beta)<!-- .element: class="fragment" -->
#### And the future of Reactivity in Angular<!-- .element: class="fragment" -->

Note: Let's kick it off with one of the things I'm most excited about personally.

----

```ts
// Imperative and non-reactive example
@Component({
    template: `
        <div>{{ count }}</div>
        <button (click)="count = count + 1">+</button>
    `
})
export class AppComponent {
    count = 0
}
```

Note: Change Detection takes care of this. Now let's make this reactive. Can't use count++ here :(.

----

```ts
// RxJS Reactive example
@Component({
    template: `
        <div>{{ count$$ | async }}</div>
        <button (click)="count$$.next(count$$.value + 1)">+</button>
    `
})
export class AppComponent {
    count$$ = new BehaviorSubject<number>(0)
}
```

Note: Has some bad practices, but for brevity and simplicity; this is to do the same thing. 

----

### Signals proposal
```ts
@Component({
    template: `
        <div>{{ count() }}</div>
        <button (click)="count.set(count() + 1)">+</button>
    `
})
export class AppComponent {
    count = signal(0)
}
```

https://stackblitz.com/edit/stackblitz-starters-ykzufn?file=src%2Fmain.ts<!-- .element: class="fragment" -->

----

### Angular's reactive primitive Signals

- Declarative<!-- .element: class="fragment" -->
- Respond to value changes<!-- .element: class="fragment" -->
- Less complex than RxJS<!-- .element: class="fragment" -->
- RxJS interop<!-- .element: class="fragment" -->

----

### Current Signal API
```ts
const count = signal(0)

count.set(2)
count.update(count => count + 1) // Update new value using existing value (map)
count.mutate(count++) // Mutate internal value

const double: Signal<number> = computed(() => count() * 2) // Keeps track of signals.

effect(() => {
    console.log(`The count is: ${ double() }`)
}) // Effect keeps track of signals()
```

----

### RxJS interop API

```ts
const count = signal(0)
const data$ = toObservable(count).pipe(
    switchMap(count => this.http.get('/api/?q='+count))
)
const data = toSignal(data$)
```

----

## Available now in v16

https://angular.io/guide/signals

https://angular.io/guide/rxjs-interop

Note: In beta in v16, hopefully becoming stable soon.

---

## Standalone all the things<!-- .element: class="fragment" -->

Note: Angular has been moving towards making things more Standalone, breaking away from the Module based opinion. See next slide for example.

----

```ts
@NgModule({
    imports: [CommonModule, ReactiveFormsModule],
    declarations: [DataComponent],
    providers: [DataService]
})
export class DataModule {}

@Component({
    selector: 'data-component',
    template: ``
})
export class DataComponent {}
```

Note: Familiar if you've been working with Angular for a while. Modules give us a neat way to group and scope. But we've had SCAMs too for a while. Lots of smaller modules.. Now, we have standalone. Doing away with that pattern.

----

Introduced in v14

```ts
@Component({
    selector: 'data-component',
    template: ``,
    standalone: true,
    imports: [CommonModule, ReactiveFormsModule],
    providers: [DataService]
})
export class DataComponent {}
```

Note: No more SCAMs, you can just have imports and providers on your standalone component and use it as such. No more modules required.

----

### Since v15.2: Migration!

```cli
ng generate @angular/core:standalone
```
<!-- .element: class="fragment" -->

https://angular.io/guide/standalone-migration<!-- .element: class="fragment" -->

Note: To migrate your application/library to standalone. Converts components, directives, pipes to SA. Does away with NgModules, updates the bootstrap.

----

### Since v16

```cli
ng new --standalone
```
<!-- .element: class="fragment" -->

Clean new application! No modules.<!-- .element: class="fragment" -->

----

- Smaller applications
- Less overhead
- Easier developer experience.

https://angular.io/guide/standalone-components<!-- .element: class="fragment" -->

Note: Easier DX, easier to learn. But doing away with a opinion and practice they've used for years. For apps that have been in development for a bit, might take a while to adapt.

---

## Non-destructive app hydration<!-- .element: class="fragment" -->


Note: What does that mean? Let's dive into what it improves upon first.

----

<img src="assets/universal.svg" style="height: 15vh;" />

### Angular Universal

For creating SSR applications<!-- .element: class="fragment" -->

```cli
ng add @nguniversal/express-engine
```
<!-- .element: class="fragment" -->

Since v16 also supports standalone apps.<!-- .element: class="fragment" -->

Note: Angular Universal from Universal Javascript; Javascript applications that runs in more environments than the browser. By adding the Express-engine to your Angular app, it can now boot in NodeJS on your servers and render an application. (And psst, looks like they're renaming it to @angular/ssr)

----

## Since v16
Full app, non-destructive hydration

- No content flickering<!-- .element: class="fragment" -->
- Future-proof architecture<!-- .element: class="fragment" -->
- Easy integration with existing apps<!-- .element: class="fragment" -->
- Up to 45% improvement of LCP<!-- .element: class="fragment" -->

Note: - Previous SSR, content flicker on hydrate. - Future-proof architecture: partial hydration and fine-grained loading of code. - LCP, largest contentful paint, 

----

```ts
import {
  bootstrapApplication,
  provideClientHydration,
} from '@angular/platform-browser';

...

bootstrapApplication(RootCmp, {
  providers: [provideClientHydration()]
});
```

Note: Granted that you're already using an Angular Universal setup, using the ng add from previous.

---

## Better developer tooling & DX<!-- .element: class="fragment" -->

----

## Vite + ESBuild<!-- .element: class="fragment" -->

- Vite for dev, esbuild for production builds<!-- .element: class="fragment" -->
- 72% improvement cold production builds<!-- .element: class="fragment" -->
- No HMR at the moment in Vite<!-- .element: class="fragment" -->
- Developer Preview in v16<!-- .element: class="fragment" -->
- Tackle i18n support before stable<!-- .element: class="fragment" -->

Note: Vite and ESBuild replaces Webpack, where Vite is now only used for dev-server. HMR is planned for the future.

https://youtu.be/kwUfeWe7DCw <-- Check this one again.

----

### Opt in Today (v16)

```json:angular.json
// angular.json
...
"architect": {
  "build": {                     /* Add the esbuild suffix  */
    "builder": "@angular-devkit/build-angular:browser-esbuild",
...
```

https://angular.io/guide/esbuild<!-- .element: class="fragment" -->

----

## Jest & Web Test Runner<!-- .element: class="fragment" -->

- Experimental Jest support<!-- .element: class="fragment" -->
- In the future; migrate to Web Test Runner<!-- .element: class="fragment" -->

```ts
npm install jest --save-dev

// angular.json
"test": {
          "builder": "@angular-devkit/build-angular:jest",
          "options": {
            "tsConfig": "tsconfig.spec.json",
            "polyfills": ["zone.js", "zone.js/testing"]
          }
        }

```
<!-- .element: class="fragment" -->

Note: ng new app: Karma was made for AngularJS and has been deprecated. Moving towards (community supported) Jest and later WTR.

----

## Injectable DestroyRef<!-- .element: class="fragment" -->

- OnDestroy now injectable in v16<!-- .element: class="fragment" -->
- takeUntilDestroyed()<!-- .element: class="fragment" -->

```ts
import { takeUntilDestroyed } from '@angular/core/rxjs-interop';

@Component({})
export class AppComponent 
    http.get('…').pipe(takeUntilDestroyed());
    destroyRef = inject(DestroyRef);

    this.destroyRef.onDestroy(() => { /* cleanup */ } );
}

```
<!-- .element: class="fragment" -->

---

## And more!<!-- .element: class="fragment" -->

### Since v16<!-- .element: class="fragment" -->

----

### Required Inputs

```ts
@Component(...)
export class App {
  @Input({ required: true }) title: string = '';
}
```

Note: Compiler time runtime inputs.

----

### Router data as Inputs

```ts
const routes = [
  {
    path: 'about',
    loadComponent: import('./about'),
    resolve: { contact: () => getContact() }
  }
];

@Component(...)
export class About {
  // The value of "contact" is passed to the contact input
  @Input() contact?: string;
}
```

Note: Route data (resolvers, data properties), path & query parameters, can now be passed as inputs. Resolve, contact -> @Input() contact

----

### Self closing tags

```
<super-duper-long-component-name [prop]="someVar"/>
```

Note: Where you normally had to < tag ></ tag >, you can now make it self closing, woo!

---

# v17<!-- .element: class="fragment" -->
### November<!-- .element: class="fragment" -->

🚨<!-- .element: class="fragment" -->

Note: Speculation zone! But of course all the things we talked about to become stable in some form.

----


### Partial & Resume

> We’re very **partial** to all of these changes and we certainly plan to **resume** this direction of where Angular is going with server side rendering, and there’s a lot more ahead with v17 and beyond. Stay tuned.
<!-- .element: class="fragment" -->

<small>https://blog.angular.io/whats-next-for-server-side-rendering-in-angular-2a6f27662b67</small><!-- .element: class="fragment" -->

Note: Partial resumability? Much like Qwik does (Qwik.io), where the app is pregenerated on the server and the full state is serialized and shipped as part of the HTML. Instant load, instant resumable. Note, the article did italicize the two words for emphasis.

----

### Component-level code-splitting APIs

> A common problem with web apps is their slow initial load time. A way to improve it is to apply more granular code-splitting on a component level. We will be working on more ergonomic code-splitting APIs to encourage this practice.
<!-- .element: class="fragment" -->

<small>https://angular.io/guide/roadmap</small><!-- .element: class="fragment" -->

Note: Tying in with the Partial & Resume part. 

Being able to tell the compiler what parts to ship instantly and which not, as a native function within Angular, would make it easier for SSR and even regular apps to ship less initial JS. How they are planning on doing this, that's not clear yet!

Fireship made a nice vid on it: https://youtu.be/kwUfeWe7DCw

----

### Micro frontend architecture

> We understood and defined the problem space for the past couple of quarters. We will follow up with a blog post on best practices when developing apps at scale. The project got delayed due to the prioritization of other initiatives.
<!-- .element: class="fragment" -->

<small>https://angular.io/guide/roadmap</small><!-- .element: class="fragment" -->

Note: The Angular team has acknowledged the idea and technological feasibility of microfrontends, but there's only unofficial solutions now with debatable stability. It'd be nice if the Angular team would throw their opinion into the mix.

----

### Control Flow & Deferred Loading

```ts
@defer on viewport {
  @main {
    <!-- this block will be deferred until the placeholder is visible -->
    @if showBody {
        <ul>
        @for item of item; track item.id {
            <li>{{ item.name }}</li>
        } @empty {
            <li>No items...</li>
        }
        </ul>
    } @else if showSummary {
    <summary-cmp />
    } @else {
    Nothing to see here...
    }
  }
  @placeholder {
    <img src="ph.png">
  }
  @loading {
    <loading-spinner />  
  }
}
```
<!-- .element: class="fragment" -->

<small>https://github.com/angular/angular/discussions/51241</small><!-- .element: class="fragment" -->

Note: As per the discussion, they changed their minds from the initial RFComments. Proposed is above, @ symbols. 

Is this in favor of the *ngIf and *ngFor? The deferred loading is a very cool idea. Less `ng-container` and `ng-template` things!

It also ties back in with Resume & Partial & Component-level splitting perhaps, to even make it possible for lazily loaded templates and pieces of codes, as they tie in with these new APIs? Interesting stuff.

---

## Thoughts?

Note: Final thoughts, excited for the future of Angular. Following good ideas from other frameworks but still staying true to what makes Angular strong. The community is alive as ever. Cool initiatives not only being embraced by the community, but also by the Angular team. Very excited to be working with Angular and related tools in the near future!

----

## Thank you!

<div class="fade-in fragment flex">
    <div class="flex-half">
  <img src="assets/bjorn.jpg" width="100" style="border-radius:100%; display: inline-flex;" class="border">
  </div>
  <div class="flex-1">
  <h1 style="font-size: 0.9em;">Bjorn Schijff</h1>
   <small><img src="assets/x.svg" style="width: 32px; padding-right: 12px; float: left;" /> <span style="float: right; padding-top: 1rem;">@Bjeaurn</span></small><br />
  <img src="assets/codestar.svg" height="30" class="clean">
  <img src="assets/ordina.svg" height="30">
  </div>
</div>

Note: Questions?
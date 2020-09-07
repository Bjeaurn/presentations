---
title: How we automated our Angular updates
notesSeparator: "^Note:"
---

Questions during the presentation?

<img src="assets/slido-qr.png" style="height: 500px; border: none;" />

---

## How we automated<br /> our Angular updates

----

## Frontend world moves fast

Note: New versions weekly, additional tooling and libraries for your favorite project to keep up with, and your daily new framework. Although, it's been setting now for a bit. It can be distracting!

----

<p class="fragment fade-in-then-semi-out visible" data-fragment-index="0">Take away focus</p>
<p class="fragment fade-in-then-semi-out visible" data-fragment-index="1">Time consuming</p>
<p class="fragment fade-in-then-semi-out visible" data-fragment-index="2">Distracting</p>

Note: This can take away focus from the things you want to work on, take precious time out of your day and distract from the things that matter.

----

## Angular CLI <!-- .element: class="fragment" -->

🦸‍♀️ Updates packages<!-- .element: class="fragment" -->

🦸 Runs migration schematics<!-- .element: class="fragment" -->

😁 Keeps us happy!<!-- .element: class="fragment" -->

Note: The Angular CLI as our personal hero made this process a lot easier, less time consuming and made it a joy to keep up to date.

----

So much so, we automated it.

---

<div class="fragment fade-up">
<div style="float: left; width: 50%">
  <img src="assets/bjorn.jpg" width="100" style="border-radius:100%; display: inline-flex;"><br />
  <img src="assets/ordina-logo.svg" height="40" style="border: 0; background-color: transparent;"/>
  <img src="assets/codestar.svg" height="30" style="border: 0; background-color: transparent;">
</div>
<div style="float: left; width: 50%; text-align: left;">
<br />
  <h1 style="font-size: 0.9em;">Bjorn Schijff</h1>
  <small style="display: inline-flex;">Sr. Frontend Engineer @ Politie</small><br />
   <small><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" style="fill: #1DA1F2"><path d="M12 0c-6.627 0-12 5.373-12 12s5.373 12 12 12 12-5.373 12-12-5.373-12-12-12zm6.066 9.645c.183 4.04-2.83 8.544-8.164 8.544-1.622 0-3.131-.476-4.402-1.291 1.524.18 3.045-.244 4.252-1.189-1.256-.023-2.317-.854-2.684-1.995.451.086.895.061 1.298-.049-1.381-.278-2.335-1.522-2.304-2.853.388.215.83.344 1.301.359-1.279-.855-1.641-2.544-.889-3.835 1.416 1.738 3.533 2.881 5.92 3.001-.419-1.796.944-3.527 2.799-3.527.825 0 1.572.349 2.096.907.654-.128 1.27-.368 1.824-.697-.215.671-.67 1.233-1.263 1.589.581-.07 1.135-.224 1.649-.453-.384.578-.87 1.084-1.433 1.489z"/></svg> @Bjeaurn</small>
</div>
</div>

Note: Introduce yourself.

---

## Why automate it?

Keeps us focussed on our job<!-- .element: class="fragment fade-in-then-semi-out" -->

Active reminders<!-- .element: class="fragment fade-in-then-semi-out" -->

Cause we can!<!-- .element: class="fragment" -->

Note: Why would you want to automate this in the first place? So we don't have to think about it, when we receive active reminder. We have to spend less time doing it and in the end cause the Angular CLI enables us to automate it!

----

## Keep developers in control

Nothing gets merged automatically<!-- .element: class="fragment fade-in-then-semi-out" -->

The end result is a merge request<!-- .element: class="fragment fade-in-then-semi-out" -->

Note: We don't want to lose control of the process. We don't want any new (automated) code in our repository without a final say from a developer and an easy way to rollback.

---

## A note about other platforms 

What about React? VueJS?

<!-- TODO Talk about React, VueJS etc. and how they're probably able to achieve similar things, even in a basic form like Githubs Dependabot. 
// Remind people this talk is about Angular cause of it's excellent CLI and how it fit into our process. But that doesn't mean it can't be done for other situations!
// `npm update` or whatever can also be a fine start to automate this process.
// The other platforms is also a nice segway into different CI solutions and the requirement we lean on. This is good part into the actual code/solutions.-->

---

## So, how do you start?<!-- .element: class="fragment fade-in-then-semi-out" -->

All you need is a repository<!-- .element: class="fragment fade-in-then-semi-out" -->

----

`ng new test-app`

or an existing app

Note: This could be either a clean repository just for testing, or an existing; as you really don't have to touch the architecture of the app to try.

---

<img src="assets/ci/github-actions.svg" style="border: none; background: transparent; height: 14rem; color: white;" />
<img src="assets/ci/Circle-CI-Logo.png" style="border: none; background: transparent; height: 14rem; filter: brightness(400%);" />
<img src="assets/ci/1200px-Jenkins_logo.svg.png" style="border: none; background: transparent; height: 14rem;" />
<img src="assets/ci/gitlab-ci-cd-logo_2x.png" style="border: none; background: transparent; height: 14rem;" />

---

# Code highlight example for future reference

<pre><code data-line-numbers="|3,8-10|9|10-12">
<table>
  <tr>
    <td>Apples</td>
    <td>$1</td>
    <td>7</td>
  </tr>
  <tr>
    <td>Oranges</td>
    <td>$2</td>
    <td>18</td>
  </tr>
</table>
</code></pre>

---

# Questions?<!-- .element: class="fragment" -->

<img src="assets/slido-qr.png" style="height: 300px; border: none;" /><!-- .element: class="fragment fade-up" -->

Note: Wait for a minute, check questions and maybe move that over? Or keep the slides.

---

## Thank you!<!-- .element: class="fragment fade-in"-->

<div style="float: left; width: 40%">
  <img src="assets/bjorn.jpg" width="100" style="border-radius:100%; display: inline-flex;"><br />
  <img src="assets/ordina-logo.svg" height="40" style="border: 0; background-color: transparent;"/>
  <img src="assets/codestar.svg" height="30" style="border: 0; background-color: transparent;">
</div>
<div style="float: left; width: 60%; text-align: left;">
<br />
  <h1 style="font-size: 0.9em;">@Bjeaurn</h1>
  <p>Please tweet me your thoughts!</p>
</div>
---
title: How we automated our Angular updates
notesSeparator: "^Note:"
---

Questions during the presentation?

<img src="assets/slido-qr.png" style="height: 500px; border: none;" />

---

## How we automated<br /> our Angular updates

----

## (Frontend) World moves fast

Note: New versions weekly, additional tooling and libraries for your favorite project to keep up with, and your daily new framework. Although, it's been setting now for a bit. It can be distracting!

----

## Where it all began

Note: So, about a year and half ago, I was working in a team and we noticed that almost every sprint we lost some time to just updating our apps. 

----

## Angular CLI <!-- .element: class="fragment" -->

🦸‍♀️ Updates packages<!-- .element: class="fragment" -->

🦸 Runs migration schematics<!-- .element: class="fragment" -->

😁 Keeps us happy!<!-- .element: class="fragment" -->

Note: We use the Angular CLI for most of our upgrades pre-work anyway, automatic migrations and packages get ready. Then we gotta test if everything still works as expected, are there any compilation errors etcetera.

----


# 💡

 Note: So we had an idea. Why not let a bot do that and we review the results?

----

## Blogged about it

https://medium.com/codestar-blog/how-we-automated-our-angular-updates-9790212aa211

Note: So I blogged about our experiences, and what it brought us. And now we are here! This showed a much more complex Jenkins example which we'll review later.

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

## Why did we want to automate?

<p class="fragment fade-in-then-semi-out visible" data-fragment-index="0">Take away focus</p>
<p class="fragment fade-in-then-semi-out visible" data-fragment-index="1">Time consuming</p>
<p class="fragment fade-in-then-semi-out visible" data-fragment-index="2">Distracting</p>

Note: This can take away focus from the things you want to work on, take precious time out of your day and distract from the things that matter.

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

<img src="assets/angular-overview-clean.png" style="float: right; height: 50%;"/>

Note: This could be either a clean repository just for testing, or an existing; as you really don't have to touch the architecture of the app to try.

---

<img src="assets/ci/github-actions.svg" style="border: none; background: transparent; height: 14rem; color: white;" />
<img src="assets/ci/Circle-CI-Logo.png" style="border: none; background: transparent; height: 14rem; filter: brightness(400%);" />
<img src="assets/ci/1200px-Jenkins_logo.svg.png" style="border: none; background: transparent; height: 14rem;" />
<img src="assets/ci/gitlab-ci-cd-logo_2x.png" style="border: none; background: transparent; height: 14rem;" />

Note: And you need a CI solution of your choice. 

----

Scheduled jobs<!-- .element: class="fragment fade-in-then-semi-out" -->

Separate pipeline<!-- .element: class="fragment fade-in-then-semi-out" -->

A way to create Pull Requests and run existing pipelines<!-- .element: class="fragment fade-in-then-semi-out" -->

Optional: A way to signal developers<!-- .element: class="fragment fade-in-then-semi-out" -->

Note: We can use anything as long as we have the above. We'll focus on GitHub Actions today mainly, as an example.

---
---

<img src="assets/angular-overview.png" style="height: 90%;" />

---

<img src="assets/ci/gha/get-started.png" />

Note: I like Github Actions, same code repo; no external integrations. This is why primary example, same goes for GitLab. We start by creating a new action.

----

<img src="assets/ci/gha/continuous-only.png" />

Note: Or if you already have CI sorted, you add a new pipeline/workflow.

---

<pre><code data-line-numbers="|7-8|9">
name: Automated Updates

# Controls when the action will run. Triggers the workflow on push or 
# pull request events but only for the main branch
on:  
  schedule:
    - cron: "0 0 * * SUN"
  workflow_dispatch: # So we can manually trigger it from the Actions page.
</code></pre>

<img src="assets/ci/gha/manual-trigger.png" class="fragment" />

Note: We create a new github action: I called mine "Automated Updates". It triggers on a cron schedule (every sunday at 00:00), and has a workflow_dispatch which is a GHA trick to be able to manually run it from the UI.

----

<pre><code data-line-numbers="">
# A workflow run is made up of one or more jobs that 
# can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that 
    # will be executed as part of the job
    steps:
</code></pre>

Note: We setup the basic build, this is very familiar for most CI solutions. They may have different names for things, but they mean the same concept basically.

----

<pre><code data-line-numbers="|2|4-5|7-11">
      - uses: actions/checkout@v2
     
      - name: Install
        run: npm install
      
      - name: Check for updates and run
        id: update
        run: |
          npx ng update --all --force
          echo ${{ toJson(steps.update.outputs) }}
</code></pre>

Note: This is the nitty gritty of our solution. We --all and --force our updates nowadays, cause compilation errors with Typescript upgrades etc. will prevent CI from completing. It'll run all migrations too.

----

<pre><code data-line-numbers="|6-13|2-4|10,11,12|">
- name: Set current date/week information
  id: date
  run: echo "::set-output name=week::$(date +%V)"

- name: Create branch and make pull request on changes
  uses: peter-evans/create-pull-request@v3
  with:
    token: ${{ steps.generate-token.outputs.token }}
    branch: "ng-update-week-${{ steps.date.outputs.week }}"
    commit-message: "chore(ng-update): Automatic updates from week ${{ steps.date.outputs.week }}"
    title: "Automatic updates from week ${{ steps.date.outputs.week }}"
    labels: updates, angular
</code></pre>

Note: So we create a branch and make a pull request on changes. Shout out to peter-evans! We use a cool little Shell trick to get the weeknumber in the `week` variable and save it in the `date` step. We use that then in the branch name, the commit message and the PR title.

---

## That's the basic concept! <!-- .element: class="fragment semi-fade-out" -->

### But there's more <!-- .element: class="fragment" -->

----

<pre class="fragment"><code>
- uses: tibdex/github-app-token@v1
  id: generate-token
  with:
    app_id: ${{ secrets.GHA_ID }}
    private_key: ${{ secrets.GHA_AUTOMATION }}
</code></pre>
<pre><code data-line-numbers="|9">
- name: Set current date/week information
  id: date
  run: echo "::set-output name=week::$(date +%V)"

- name: Create branch and make pull request on changes
  uses: peter-evans/create-pull-request@v3
  with:
    token: ${{ steps.generate-token.outputs.token }}
    branch: "ng-update-week-${{ steps.date.outputs.week }}"
    commit-message: "chore(ng-update): Automatic updates from week ${{ steps.date.outputs.week }}"
    title: "Automatic updates from week ${{ steps.date.outputs.week }}"
    labels: updates, angular
</code></pre>

Note: Some of you might have noticed that there was a line I didn't explain. This one was added later when I found out the GHA does not by default run bot-generated Pull Requests with the regular CI cycle, it doesn't count as "on: push or pull request". But somebody smart found a way to do that, using another Github tool.

----

<img src="assets/ci/gha/gha-app/gh-apps.png" class="fragment" />

<img src="assets/ci/gha/gha-app/my-gh-app.png" class="fragment" />

----

<img src="assets/ci/gha/gha-app/secrets.png" />

----

<pre><code data-line-numbers="|5-6|3,11|">
- uses: tibdex/github-app-token@v1
  id: generate-token
  with:
    app_id: ${{ secrets.GHA_ID }}
    private_key: ${{ secrets.GHA_AUTOMATION }}

    ...

  with:
    token: ${{ steps.generate-token.outputs.token }}
</code></pre>

----

And that makes GitHub Actions treat our pull request like a properly pushed one.

<div class="fragment">
  Thanks Peter Evans!<br />
  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" style="fill: #1DA1F2"><path d="M12 0c-6.627 0-12 5.373-12 12s5.373 12 12 12 12-5.373 12-12-5.373-12-12-12zm6.066 9.645c.183 4.04-2.83 8.544-8.164 8.544-1.622 0-3.131-.476-4.402-1.291 1.524.18 3.045-.244 4.252-1.189-1.256-.023-2.317-.854-2.684-1.995.451.086.895.061 1.298-.049-1.381-.278-2.335-1.522-2.304-2.853.388.215.83.344 1.301.359-1.279-.855-1.641-2.544-.889-3.835 1.416 1.738 3.533 2.881 5.92 3.001-.419-1.796.944-3.527 2.799-3.527.825 0 1.572.349 2.096.907.654-.128 1.27-.368 1.824-.697-.215.671-.67 1.233-1.263 1.589.581-.07 1.135-.224 1.649-.453-.384.578-.87 1.084-1.433 1.489z"/></svg> @peterevans0
</div>

---

## Handling Error situations
<!-- TODO Advanced part 2? Speed up examples now, show Slack call from GHA? Detecting a fail? -->
<!-- Main concept here is that all of these concepts and ideas can be applied to many different CI solutions.-->
<!-- You just need to find what works for you, your team and your situation. -->

----

### Backstory

Example from blogpost<!-- .element: class="fragment" -->

<img src="assets/ci/1200px-Jenkins_logo.svg.png" style="height: 20rem; border: none;" class="fragment" />

Disclaimer!<!-- .element: class="fragment" -->

Note: So let me tell you a bit about Jenkins, and the original example that spawned the blogpost. DISCLAIMER: Examples may be a bit outdated, and were written at the time with limited knowledge about Jenkins. Maybe there's easier ways to achieve similar results!

----

<pre><code data-line-numbers="|3-5|7-8|10,12-13|16|18,20|23|28|33|44-45|56-59-61|">
        // Read the blogpost to see how we got the ${allCommands}!
        sh "node_modules/@angular/cli/bin/ng update ${allCommands} 2>&1 | tee update-result.txt"
            }
        }

        def lastLine = sh script: 'tail -n 1 update-result.txt', returnStdout: true
                 def repoIsDirty = sh script: "git status --porcelain | grep '^.[^ ]' > /dev/null", returnStatus: true
         
                 if (repoIsDirty != 0) {
                     echo lastLine
                     echo "" + lastLine.indexOf('Incompatible')
                     if (lastLine.indexOf('Incompatible') >= 0) {
                         String updateResult = readFile('update-result.txt').trim()
                         String report = "Automatic Update failed. See output:\n```\n$updateResult\n```"
                         rocketSend channel: rocketChatChannel, emoji: ':cry:', message: report, rawMessage: true
                         error 'Incompatible changes detected, cannot complete update automatically'
                     } else {
                         echo 'Nothing has to be updated! \\o/'
                         rocketSend channel: rocketChatChannel, emoji: ':yeah:', message: 'No outdated dependencies :yeah:', rawMessage: true
                     }
                 } else {
                     updatesApplied = true
                 }
         
             }
         
             if (updatesApplied) {
                 conditionalStage('Updating Git') {
         
                     sshagent(credentials: [env.GITLAB_CREDENTIALS_ID]) {
                         // Do git config stuff for name and email
                         // See how much more complex it was for us to get a simple branch pushed?
                         sh "git add -A && git commit -m 'Automatic update of dependencies by ng update $currentDate'"
                         sh "git push -u origin $targetBranch"
                     }
         
                     def commitId = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
                     echo "Building commitId $commitId on branch $targetBranch"
                     sh "git clean -dfx"
                     sh "git log --oneline -1 | sed 's/^.*: //'"
                 }
         
                 conditionalStage('Create MR') {
                     withCredentials([[$class          : 'UsernamePasswordMultiBinding', // blabla credentials) {
                         def getProjectResponse = stepGeneral.gitlab_getProjectDetails(token)
                         def projectId = stepGeneral.parseJsonText(getProjectResponse.content).id
                         def postBody = """
         							{
         							"id": "$projectId",
         							"source_branch": "$targetBranch",
         							"target_branch": "master",
         							"title": "chore(ngUpdate): Automatic upgrade by Jenkins"
         							}
         						"""
                         def mrUrl = env.GITLAB_API_BASE_URL + "/projects/${projectId}/merge_requests"
         
                         // Perform tag
                         def response = httpRequest contentType: 'APPLICATION_JSON', httpMode: 'POST', requestBody: postBody, customHeaders: [[name: 'PRIVATE-TOKEN', value: token]], url: mrUrl
                         def webUrl = stepGeneral.parseJsonText(response.content).web_url
                         rocketSend channel: rocketChatChannel, emoji: ':yeah:', message: "Auto Updater: New merge request ${webUrl}", rawMessage: true
                     }
                 }
    }
</code></pre>

😰<!-- .element: class="fragment" -->

Note: This is the complexity we had to deal with in Jenkins to get our conditional reporting. Again, disclaimer; a while ago with limited knowledge. The whole pushing and creating a request is so much harder without the plugin we used.

----

Way easier in GitHub Actions

<pre class="fragment"><code>
- name: Do things on failure
  if: {{ failure() }}
  run: echo 'This is run cause our build failed'
</code></pre>

The pullrequest plugin sets a variable if a PR was made.<!-- .element: class="fragment" -->

Note: We can use the PR var to send a message with the PR number, or send a message if no updates were made (no PR + no fail = no updates)

---

## Adding a Slack notification

----

<img src="assets/ci/slack/gha-slack.png" />

Thanks to Ilshildur.

----

<pre><code>
- name: Slack notification
  env:
    SLACK_WEBHOOK: ${{ secrets.SLACK_WEBHOOK }}
    SLACK_USERNAME: ThisIsMyUsername # Optional. (defaults to webhook app)
    SLACK_CHANNEL: general # Optional. (defaults to webhook)
    SLACK_AVATAR: repository # Optional. can be (repository, sender, an URL)
  uses: Ilshidur/action-slack@2.0.2
  with:
    args: 'A new commit has been pushed.' # Optional
</code></pre>

----

<img src="assets/ci/slack/activate-webhooks.png" />
<img src="assets/ci/slack/setup-bot.png" />

----

<img src="assets/ci/slack/add_slack_secret.png" />

----

<img src="assets/ci/slack/first-attempt.png" />

----

<pre class="fragment"><code data-line-numbers="5,9-10,3">
- name: Slack notification
  if: ${{ steps.create-pr.outputs.pull-request-number }}
  env:
    SLACK_WEBHOOK: ${{ secrets.SLACK_WEBHOOK }}
    SLACK_USERNAME: PR-bot # Optional. (defaults to webhook app)
    SLACK_AVATAR: repository # Optional. can be (repository, sender, an URL)
  uses: Ilshidur/action-slack@2.0.2
  with:
    args: "An automatic update has resulted in a Pull Request. 
      https://github.com/Bjeaurn/automating-updates/pull/${{ steps.create-pr.outputs.pull-request-number }}"
</code></pre>

<small class="fragment">`${{ steps.create-pr.outputs.pull-request-number }}`</small>

<img class="fragment" src="assets/ci/slack/much-better.png" />

----

# 👍

Don't like Slack? <!-- .element: class="fragment" -->

<div style="width: 100%; float: left;">
<img src="assets/ci/slack/dont-like/mattermost.png" class="fragment" style="width: 25rem; float: left;" />
<img src="assets/ci/slack/dont-like/rocketchat.png" class="fragment" style="width: 25rem; float: right;"   /><br />
</div>
<div style="width: 100%; float: left;">
<img src="assets/ci/slack/dont-like/webhook.png" class="fragment" style="width: 25rem; float: left;" />
<img src="assets/ci/slack/dont-like/email.png" class="fragment" style="width: 25rem; float: right;"  />
</div>

Note: Nice! But what if you don't like Slack? Well a quick search gave us actions for almost every other imaginable alternative that uses a Webhook. Even send an email!

---

<!-- TODO - This slide is the summary and inspiration part. Benefits, gains, reasons why you should too. --->
## What did we gain?

Keeps us focussed on our job<!-- .element: class="fragment fade-in-then-semi-out" -->

Active reminders<!-- .element: class="fragment fade-in-then-semi-out" -->

Cause we can!<!-- .element: class="fragment" -->

Note: Now, we don't have to think about it, when we receive active reminders. We have to spend less time doing it and in the end cause the Angular CLI enables us to automate it! And we just review the work after the bot reports it.

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
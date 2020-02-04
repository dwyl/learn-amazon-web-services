# Context

Let's talk about (sending|receiving) **`email`**. 💬
In the _past_ we have taken a very "MVP" approach to sending email in both our [`Node`](https://github.com/dwyl/sendemail) & [`Elixir`](https://github.com/dwyl/learn-phoenix-framework/blob/master/sending-emails.md) Apps.
(_I use that term "**MVP**" **euphemistically** to mean "**bare minimum**" not really "**viable**" beyond testing the initial idea..._ 🙄)

We have included the email _functionality_ in apps (_e.g: "Welcome to the App!", "confirm your address", "reset your password", etc._) as a [box ticking exercise](https://dictionary.cambridge.org/dictionary/english/box-ticking) not as a _core_ UX/UI of the App.  I feel that we have been letting _ourselves_ and the people _using_ our App(s) down by not _focussing_ on the UX of the emails we _send_. Sending out **_unremarkable_** emails that have a **_low_ open/click-through rate** (_which we don't actively **monitor!**_) or having an inbox full of unanswered email (_because we **lack** a **proper system** for prioritising/handling inbound emails_)  means all our other efforts building great features for our App(s) is _wasted_! If people aren't engaging with the **`email`** they are by _definition_ not engaging with the App. 😞

### Restaurant Analogy: 🥣

Imagine you're a [**Chef**](https://pt.wikipedia.org/wiki/Chef) who has practiced for _years_ to make **great food**. You have spent _most_ of your life savings to build a **restaurant** where you have paid great attention to detail with the menu/food, service and interior decor. But on the _outside_ of the restaurant, there's no sign/marking and the front door is through a **_dimly_ lit alleyway** so it looks _abandoned_.

![image](https://user-images.githubusercontent.com/194400/73617736-cfa44f00-4619-11ea-9edd-91cb8b21122a.png)

In this analogy the ***interface*** to your **_delicious_ food** is the **_dimly_ lit alleyway** which means you aren't going to get very many _customers_. Should you continue to focus on improving the _inside_ of the restaurant  (_e.g: the food and service quality_) hoping that people will _ignore_ the alley? ***Or*** attempt to "fix" the "**UX**" of the alleyway by investing in some _lights_, signs and decorations?

![image](https://user-images.githubusercontent.com/194400/73617903-4e4dbc00-461b-11ea-8a5f-28eb91bfbb1b.png)

_After_ you have made an effort to improve the "**UX**" of the alleyway, you can get back to running your business. But you still need to _maintain_ the alleyway as the _interface_ to your product.

If we apply this analogy to building a Software App/Product, the ***inside*** of the restaurant (_menu, food, service, etc._) are the ***features*** of the Product. The ***outside*** of the restaurant, the **alleyway** and front door are the [**_barrier_ to adoption**](https://www.uxmatters.com/mt/archives/2010/11/barriers-to-adoption-and-how-to-uncover-them.php). If the Chef (_budding restaurateur_) does not treat the ***interface*** to their restaurant as a ***feature*** of their **product**, then it does not _matter_ how good the food is.

> This past week, I've been _painfully_ reminded of how _horrible_ the **UX** of a ***`noreply`*** **`email`** is. You basically feel like nobody at the company _cares_ about you as a person. You are just a "unit" in their accounting system. https://github.com/nelsonic/nelsonic.github.io/issues/781 💔
We don't want _anyone_ using the @dwyl App to _ever_ feel _unloved_ because of a _crappy_ **`email`** UX.



# Story

**`email`** is an _integral_ part of the [***feedback loop***](https://en.wikipedia.org/wiki/Feedback) of the [App](https://github.com/dwyl/app). ♻️
Without it we have no way of connecting with people _using_ the App to welcome them to our _community_ of people focussed on maximising personal effectiveness. If people don't _open_ the **`email`** we send (_because it's unremarkable_), we end up with no way of informing them to ***`return`*** and ***use*** the App!

But its a _lot_ more than glorified notification mechanism to remind people that a task they delegated has been completed (_or has a follow up question from assignee_), **`email`** is the _interface_ people experience when they _aren't_ using the App which encourages them use it. **`email`** is a make-or-break UI/UX!

There are a _several_ stories that feature **`email`** and they all need to have **_amazing_ UX** if our product is going to be successful. Having a "so so" **`email`** experience will mean people will not want to (a) _open_ the emails we send them and (b) _interact_ with them.

I am listing the "stories"  in the order of _impact_ to both the people _receiving_ the emails and _us_ the developers/owners of the product _sending_ those emails i.e. the "bottom line". (_highest impact first_)

Each one of these stories will need to be split out into a separate issue with a checklist.

## "User" (_the person **using** the App_)

The UX of the person _receiving_ emails from our App is the _focus_ of this Epic.

### Registration > _Welcome_!

As a person registering to use the @dwyl App,
I want to receive a **_welcome_ `email`** outlining what I can _do_ in the App 💌
So that I feel _connected_ with the App.

Our **_welcome_ `email`** should have the _highest_ **open rate** in the industry.
We need to _track_ it from day 1.
Ideally we should give the person some _useful_ information in the **_welcome_ `email`**
with a **_single_ call-to-action** (CTA) that a high percentage of people _interact_ with.
https://en.wikipedia.org/wiki/Call_to_action_(marketing)



### Forgotten Password 😬

As a person who has forgotten their password and is "locked out" of the App,
I want a _seamless_ way to reset my password
So that I can get back to using the App as fast as possible.

The sense of being "locked out" is a relatively stressful experience. We need to get people back on track as quickly as possible.
The focus of this email needs to be the _link_ they need to click to reset their password. We also need to reassure people that's it's perfectly normal to forget a password.

The App should _track_ how many times a person forgets their password.
So that if it detects that a person is consistently forgetful, the password reset email should suggest a password manager for their most used device.


### _Relevant/Useful_ Notifications 🔔

As a person using the @dwyl App to focus on my most important work,
I want to receive _occasional_ **`email`** ***notifications*** with SMART insights
So that I can stay on track toward achieving my goal(s).

As soon as we have the [**_essential_ functionality**](https://github.com/dwyl/app/issues/262) for the App working,
we should determine what the most _relevant_ information is to keep people engaged with working on their goals.


We should _never_ send people irrelevant/useless notifications the way _most_ social networks do.
For the first 1k people using the App, **`email`** ***notifications*** will be the primary feedback loop.
Later on we may end up using _native_ notifications in our Native [Flutter](https://github.com/dwyl/technology-stack/issues/81) Apps and [Browser Push Notifications](https://github.com/dwyl/app/issues/220) based on demand/feedback from people _using_ the app.


### Log in 🚪

As a person _authenticating_ with a Web/Mobile App using my **`email`** address and a **`password`**,
I want to be informed by email when I log-in on a new device/platform
So that I am reassured that the App is protecting my highly personal data.

This email should _not_ be sent _every_ time a person logs in
otherwise the open rate will be very low.
We need a function that checks which device(s) have been used by the person in the past
and if a **`new`** device is used, we should email them to confirm that it is a legitimate login event.


### Newsletter  🗞

As a person wanting to get useful/relevant actionable insights on personal effectiveness,
I want to receive a monthly/fortnightly/weekly **`newletter`** a **_bitesize_ digest**
So that I can be _inspired_ to take _action_ on achieving my goal(s).

The Newsletter use-case is not a _focus_ while we are building the _features_ of the App.
But just as I expect our "Blog" to be a _significant_ source of SEO and inbound marketing.
I expect that the people who _already_ using the App to _want_ to get personal effectiveness insights/tips delivered to their inbox. This is not just a question of "repackaging" the Blog content. We need to _summarise_ it for busy people!


### _Replies_ > Conversation!

As a person using the @dwyl App and encountering challenges with (_or ideas for_) the UX/UI,
I want to be able to _reply_ to any **`email`** sent by the @dwyl App
so that with the @dwyl team via email


"noreply" / "no-reply" email addresses are a _horrible_ [UX antipattern](https://github.com/dwyl/hq/issues/468).
But _not replying_ to people's emails can be even _worse_ than a "noreply" email address!
So we _need_ to have a _system_ for handling replies from people! A system that _everyone_ in the company/team can access.  



## Developer (_the person deploying the `email` system_) 👩‍💻

As a person building/deploying a Phoenix Web App,
I want the **`email`** sent by the App to be ***remarkable***
So that people consistently engage/re-engage with the app.

Our App aims to help people be more effective with their efforts.
We want to ensure that our primary communication channel (**`email`**)
is the _best_ it can be.


### Analytics

As a data-driven developer who makes informed decisions based on usage analytics
I want to collect data on bounce, open, click through and rejection rates
So that I know how _effective_ our **`email`** system is.

Without collecting _data_ we are attempting to drive to our destination at night, without a map or head lights!



## Product Owner/Manager 👩‍🍳

As a [**Product Owner/Manager**](https://github.com/dwyl/product-owner-guide/issues/46) for promising Software Product,
I want to ensure that the **UX** of _every_ **`email`** sent by the system is ***remarkable***
So that the quality of the App is not let down by a poor feedback mechanism.

The **Product Owner/Manager** of the @dwyl App needs to be clear that they are responsible for **`email`** quality.
It's not enough to send "transactional" or "generic" email, we need each **`email`** interaction to be ***remarkable***.



# Question: _When_ Should We _Focus_ on **`email`** ? ❓

I _agree_ that we should focus on the "core" functionality/features of the app described in [#262](https://github.com/dwyl/app/issues/262). I want to propose that we _add_ **`email`** the list either in MVP or "very soon" after.




We all have a decent amount of experience of working at companies that got **`email`** _right_ and _horribly wrong_ and I want to frame the discussion from a "**Product**" ***perspective*** rather than simply a piece of "_Business as Usual_" ***functionality*** of the App. In other words we want the _quality_ of the **`email`** ***System*** we use [**`@dwyl`**](https://github.com/dwyl) to be _excellent_. 😍
We want the feeling people experience when they _see/open_ an email from _us_ to be ["_spark joy_"](https://youtu.be/9AvWs2X-bEA) ✨

> I don't want to throw shade/stones at anyone for the past lack of effort in **`email`** in previous projects, I take _responsibility_ for not lobbying past Product Managers/Owners for making it a priority. Rather I want to make the **business case** that if we don't make **`email`** _amazing_,
we are pretty much **_wasting_ our time** with building the rest of the features in the App.
Both the Product Owner/Manager and the _whole_ Dev Team need to make **`email`** a priority or we risk _failure_.

<!---
[![image](https://user-images.githubusercontent.com/194400/73759115-5add2c00-4763-11ea-8f9a-afefce70ae72.png)](https://youtu.be/MT4Ig2uqjTc)
https://youtu.be/MT4Ig2uqjTc
-->

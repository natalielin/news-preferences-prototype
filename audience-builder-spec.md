# ML Audience Builder: Product Spec

## User problems solved

| Problem today | What the builder does |
|---|---|
| Audiences depend on analysts writing SQL queries; each request waits in an analyst queue | Self-serve audiences built from model scores, ready in minutes |
| No way to tell how fresh an audience is | Every audience shows when it was last updated |
| No way to tell whether readers are engaged enough to send to safely | Engagement tiers, plus predicted complaint risk for each mailbox provider |
| No projected results | Projected reads, subscription starts and opt-outs, shown as ranges |
| Adjusting the audience only changes a count | Shows who the next readers added would be, and what they cost |
| Lifecycle stage and permission are not part of audience building | Audiences filter by lifecycle stage and by permission level (newsletter opt-in, declared interest, or inferred interest) |
| Each channel is planned separately, and email is the default for everything | One builder covers email, push, in-app and SMS, sending on one channel, on several fixed channels, or dynamically on each reader's preferred channel |
| Each send is decided alone | Shows how many urgent sends the team has left, other teams' sends to the same readers, and how long the content stays relevant |
| No record of who decided what | Every send saves a one-line decision log |
| A new audience question requires a new trait, so most time windows never get built | The person sets the event, the count, the property and the window. "Games on iOS, at least five times, within two days" is one audience, and no new trait is created |
| Traits sync overnight, so a welcome or a save offer lands a day or two late | Welcome, payment recovery, abandoned checkout and soft-cancel saves fire from the event within minutes |
| A journey has no stop, so people keep getting messages after they pay, unpause or unfollow | The stop is part of the audience. Grace recovered, pause ended, cancel revoked or unfollowed ends the sequence |
| The same fact is copied into a new model for every product, platform, game or channel | A new product, platform, game, topic or channel is a value on an event that already exists. The builder composes that value |
| Eligibility is a day or two behind the send | Marketing opt-in, a dead push token, email eligibility, first-send and emails already sent this week sit in front of the score |
| Lifecycle stage does not say what someone can buy | The audience includes entitlement, grace, Apple or Google billing, B2B and gift status, so the offer matches what they can take |
| Interest, a declared follow and product use are one control | Topic strength, a declared follow and which product they used are separate. Health and politics stay out of inference |
| Place and score freshness are unlabeled | Place is named as billing, last session or registration. Every score shows its source, its age and the fallback when it is missing or stale |
| Churn and acquisition scores are only a yes or no | "Curious" and "high churn risk" suggest a reach, and show who would be added, how fresh the score is, and the permission rule used when the score is missing |

## Competitive landscape

A review of marketing automation platforms and CDPs (Adobe, Salesforce, Braze, Klaviyo, Iterable, Hightouch, Segment, Lytics and others) shows three patterns:

1. **Plain-language audience authoring is now standard.** These tools turn a description into ordinary segment rules; the rules still define the audience.
2. **Predictive scores are standard, but few vendors prove them.** Most offer churn and conversion scores. Only a few, Hightouch, Segment, Braze, Adobe and Bloomreach, publish quality evidence such as lift, backtests or score drivers.
3. **Lifecycle decisioning is moving past fixed segments.** Braze's decisioning agents make one-to-one choices without customer segments ([Braze](https://www.braze.com/product/brazeai-decisioning-studio)). In Hightouch, the audience is just the eligible pool, and an agent chooses message, channel and timing inside it ([Hightouch](https://hightouch.com/docs/ai-decisioning/overview)).

Most scores refresh weekly, and lookalike quality depends on a large enough source audience ([Hightouch](https://hightouch.com/docs/customer-studio/lookalike-audiences)). Segment, our CDP, has transparent predictions but no native lookalikes or one-to-one decisioning ([Twilio Segment](https://www.twilio.com/docs/segment/unify/traits/predictions)).

| Pattern in the market | Seen in | How this builder adapts it |
|---|---|---|
| A score slider that shows how many people are targeted | Braze ([Braze](https://www.braze.com/docs/user_guide/brazeai/predictive_suite/predictive_churn/analytics)) | Four reach options, plus a description of the next readers added and their complaint risk |
| A reach-versus-accuracy control on predictive audiences | Lytics ([Lytics](https://docs.lytics.com/docs/leverage-lookalike-models-and-predictive-audiences)) | The Balanced reach is suggested from the goal, so most people never tune it |
| Lookalike audiences | Lytics, Hightouch, Adobe | A one-checkbox suggestion that stays within each reader's permissions |
| AI that drafts but never saves on its own | Salesforce ([Salesforce Help](https://help.salesforce.com/s/articleView?id=data.c360_a_marketing_agent.htm&language=en_US&type=5)), Adobe | Suggestions stay marked until a person saves them; goal and emphasis start blank |
| Delivery at a set time in each reader's time zone | Braze ([Braze](https://www.braze.com/docs/user_guide/messaging/campaigns/schedule_your_campaign/scheduled_delivery)), Iterable ([Iterable](https://support.iterable.com/hc/en-us/articles/14761834767508-Sending-or-Scheduling-a-Blast-Campaign)) | Geo-based timing |
| Optimal send time per user | Braze Intelligent Timing ([Braze](https://github.com/braze-inc/braze-docs/blob/develop/_docs/_user_guide/brazeai/intelligence/intelligent_timing.md)) | Reader-based timing, kept inside the window when the story is still relevant |
| Per-user channel choice | Braze, Hightouch | Dynamic sending, where a reader's declared preference always wins and the model fills in for everyone else |

**What is ours.** No vendor reviewed combines the following for a newsroom:

- inbox safety by mailbox provider, shown next to expected value;
- newsletter permission levels and news-specific lifecycle stages;
- an urgent-send allowance and overlap checks against other desks' sends;
- a flow built around editorial judgment rather than full automation.

## Competitive advantages

- **Safety and value shown side by side.** Most tools blend them into one score or keep reputation on a separate dashboard. Here, inbox safety and expected value appear next to each other at the moment of sending.
- **Reach control that describes the people being added.** Hightouch shows a reach curve ([Hightouch docs](https://hightouch.com/docs/ai-decisioning/agents)); this builder adds who the next readers are and what including them costs.
- **Risk shown per mailbox provider.** A problem at one provider stays visible instead of disappearing into an average.
- **Built for editorial judgment.** The goal and emphasis start blank, and overrides are always available. Rival tools put people at goal-setting and let AI run the execution.
- **Role-neutral.** One builder works for editors, product managers and marketers.

### Advantages still to build

The list above is what the page can show once scores exist. These are the advantages that are not built yet. Rival tools already turn a sentence into segment rules. The opening is to make those rules compose from a shared event model, with newsroom eligibility the model cannot override.

- **One event, many audiences.** A person sets the event, how many times, which property and which window. "iOS, Games, at least five times, within two days" does not require a new trait. Adding a product, game or channel is a new value, not a new model.
- **Minutes-fresh triggers, with a stop.** Welcome, grace, abandoned checkout and soft-cancel saves fire from the event, not the nightly sync. The matching stop event ends the sequence. A nightly trait cannot do either.
- **Hard eligibility in front of ranking.** Email eligible, marketing opt-in, per-app push, SMS opt-in, first-send, and emails already sent this week are gates. Apple and Google subscribers are kept out of offers those stores cannot fulfill. The model ranks only the people who pass.
- **Entitlement-aware reach.** The audience can say "plays Games and does not pay for Games," or "All Access, annual, renews within 14 days, high churn risk." Reach describes the commercial state, not only a lifecycle chip.
- **Interest with a strength and a source.** Topic engagement is High, Medium, Low or absent, with a window, and it stays separate from a declared follow (league, team, newsletter) and from product use (News app versus Games app). Sensitive topics are not filled in by inference.
- **A sequence, not only a blast.** "Subscribed in the last 7 days, then did not open the app" is an audience. Journeys are built from that shape.
- **A contract on every signal.** Each input shows its owner, whether it is minutes-fresh or batch, and the fallback when it is missing or older than its window. Volume is visible before a new trait is published, so a self-serve audience does not quietly create a new Segment model.

## Human in the loop

People make the choices that carry judgment: goal, emphasis, final approval, and any override. The model makes the choices that need more data than a person can hold: ranking readers, scoring risk, and picking the best send time. Model choices arrive as suggestions until a person confirms them. Every send records who decided.

What a person still has to do once audiences are composed from events and traits:

- **Compose the question.** The person picks the event, the count, the property, the window, the trait conditions and any sequence ("did this, then did not do that"). The model does not invent the audience from a goal alone.
- **Decide what may be inferred.** Declared follows, newsletter opt-ins and product entitlements are facts. Interest strength may be suggested. Health, politics and any other sensitive topic stay out of inference unless a person explicitly includes a declared or rules-based value.
- **Keep eligibility outside the model.** Opt-in, a dead push token, bounce, suppression, first-send and store billing (Apple, Google) are rules. A higher score cannot put those readers back in.
- **Own the stop.** A person confirms the paired event that ends a journey. The model may rank who is worth a save offer. It does not decide that a recovered payment should keep getting dunning.
- **Treat a coarse score as a suggestion.** "Curious" and "high churn risk" can pre-select a reach. The person sees the source, the age and the fallback, and can clear the suggestion. A missing or stale score falls back to the permission rule, not to a guess.
- **Leave publishing to the owning team.** Audience operators combine what already exists. A new event or trait is published by its owner (Commerce, Games, Auth, Messaging, and so on), with a name, a freshness window and an expected volume. The builder does not create a trait as a side effect of a send.

What ML audiences add over SQL audiences:

- **Freshness:** scores refresh on a schedule (daily target) instead of when an analyst gets to a ticket.
- **Predictive quality:** readers are ranked by likelihood to read, so the same number of sends produces more engagement.
- **Safety optimization:** readers likely to complain or lose interest are flagged before the send, so complaint rates stay under the Gmail lines of 0.1% (recommended) and 0.3% (never reach) ([Gmail Help](https://support.google.com/a/answer/14229414?hl=en)).
- **Discovery:** readers with a real interest that no rule captured are found and labeled "Inferred."
- **Preferred channel:** in Dynamic mode, each reader gets their declared channel, or else the channel where they are most likely to engage and least likely to opt out, within their permissions.
- **Best time per reader:** Reader-based timing sends at each reader's usual reading time, within the window when the content is still relevant.
- **Measured learning:** holdout tests and projected-vs-actual comparisons improve the model over time.
- **Efficiency:** analysts move from writing queries to setting goals and validating models.

## Audience inputs

What the page can filter on today:

- **Lifecycle stage:** five groups on screen (Registered, Subscribers, Former subscribers, Churned, Anonymous). "All 10 stages" reveals the detailed stages within each group. Churned · dormant is flagged for high complaint risk and sent last in waves. Anonymous readers are reachable by push and in-app only.
- **Permission:** Newsletter opt-in (the default for newsletter sends), Declared interest, or Interest including inferred. Each level widens reach and adds risk.
- **Channel preference:** any channel a reader has explicitly said they prefer always wins. ML only chooses among the channels each reader has opted in to.

What the audience has to be able to take in. These are the inputs the Standard Traits model makes available. The builder should compose them. It should not require a new trait for each combination.

| Input | What the person sets | Examples | Freshness |
|---|---|---|---|
| Event | What happened, how many times, on which property, inside which window | Session on Games, iOS, at least 5 times, within 2 days. Registered on News. Subscription started. Puzzle completed. | Minutes. This is what makes a welcome immediate. |
| Sequence | Did this, then did not do that | Subscribed within 7 days, then no session. Grace entered, then not recovered. Payflow started, then no subscription. | Minutes. The stop event is part of the audience. |
| Current state | What is true now, not a search through history | User type: Anonymous, Registered, Subscriber, Former. Former split into paid and free. Entitlement list. Bundle: All Access, bundle, standalone. Payer: self, gift, share, B2B, third party. Billing cadence and system. In grace, pending cancellation, in trial. | Fast where the state changes the next send (subscription flags, marketing opt-in, push reachable, a game streak). Hourly to daily for slow facts (price, billing country, registration date). |
| Product use | Which product and platform, in a window the person chooses | News, Games, Cooking, The Athletic, Wirecutter, Audio, on app or web. The window is not limited to 7, 30, 60 and 90. | Daily batch is enough for habit. The event is used when the window is "the last few hours." |
| Interest | Strength of reading, kept apart from a follow | High (30 days), Medium (31–60), Low (61–90), or absent. Politics, world, opinion, business, arts, style, health, science, food, books, climate, housing, travel. Sports reading is separate from a declared Athletic league. | Daily. Health and politics are not inferred to fill a gap. |
| Declared preference | Only what the person said | Newsletter subscriptions. Followed leagues and teams. Push opt-in per app. SMS opt-in. In-app opt-in. | Opt-in and push reach are minutes-fresh. A stated channel still wins. |
| Eligibility | Gates in front of any score | Email address eligible. Marketing opt-in. Acquisition versus engagement eligibility. First send still owed, and for which product. Emails already sent in 7 days. | Opt-in and bounce cannot wait for a nightly job. |
| Place and time | Which geo, named by source | Billing address, last session, or registration. Session timezone for send time. | Last session is the current place. Registration never changes. Billing is the stable commercial place. |
| Model tier | A suggestion, with a fallback | Curious (acquisition). High churn risk. Games habit and churn cohorts. | Shown with source and age. Missing or older than the window falls back to the permission rule. |

Dates are stored as dates (term end, next bill, card expiry). The builder compares them with today. A "days until renewal" trait would have to be recomputed every night for every reader.

## Multi-channel

Channel choice and content share one Message step, laid out as a short decision tree.

**1. Send on**, with three options:

- **Single channel.** Everyone gets the message on one channel. Suits channel-specific products, such as a newsletter that must arrive by email.
- **Multiple channels.** Everyone eligible gets the message on every selected channel. The step shows how many readers would get more than one message, and those extra messages count toward frequency limits. Suits high-stakes sends.
- **Dynamic** (suggested). Each reader gets exactly one message, on their preferred channel, chosen from the allowed channels. The preference can come from one of two sources:
  - **Declared only:** readers who have stated a preference get that channel; everyone else gets email.
  - **Declared, then predicted:** a stated preference still wins, and the model predicts the best channel for everyone else.

  The step shows how readers split across channels and the expected change in reads compared with email only.

**2. Content.** One card appears for each channel in use:

- **Email** is the source. It shows the subject line, with checks for preview text and that the lead story is fresh.
- **Push** comes pre-filled with a suggested draft based on the email: a title under 50 characters plus one supporting line. The person chooses "Use draft" or edits it first.
- **In-app** reuses the email's lead story card automatically.
- **SMS** is written by hand, with a 160-character count.

Timing adds quiet hours for push and SMS. Review shows risk by channel, using each channel's own measure:

- Email: complaint rate by mailbox provider
- Push: disable rate
- SMS: opt-out rate
- In-app: dismiss rate

## How ML makes this easier

The builder exposes about ten settings, but only two require a person's judgment every time: **Goal** and **Emphasis**. The model pre-fills the rest, and one "Use suggestions" click accepts them all.

| Without ML | With ML |
|---|---|
| Write or request a SQL query for each audience | Choose a goal; a scored audience is ready |
| Guess a reach size, then see a count | Reach comes pre-set at the point where adding more readers stops paying off, with the next readers described |
| One channel and one send time for everyone | The model chooses a channel and a send time for each reader, within that reader's permissions |
| Check safety after the send, on dashboards | Risk is predicted per channel and per provider before sending |
| Audiences go stale between query runs | Scores refresh automatically |
| Results reviewed ad hoc | Every send is compared against projections and a holdout, and suggestions improve over time |

More settings do not mean more work. Each setting is a place where a person can step in, not a box they must fill. Experts can open "All 10 stages" or switch Message to "Single channel." Everyone else accepts the suggestions and spends their attention on the goal, the emphasis and the message.

## Functional requirements

Requirements cover three areas: the product, the ML models behind it, and an AI layer on top. In each area, the needs are fixed: what the builder must show, and the limits it must respect. The methods, targets and sequencing are starting points for the ML and AI teams to shape, and each table ends with the questions those teams are best placed to answer.

### Product

| Epic | Scope | Milestones |
|---|---|---|
| 1. Builder page | One page with six steps: Goal, Audience, Emphasis, Message, Timing, Review. Each step has a Save button. All steps are open by default, with Collapse available. Goal and Emphasis start blank. Audience, the Message channel choice, and Timing are marked Suggested until saved. One "Use suggestions" action accepts every suggested step at once. | M1: the page and its steps. M2: earlier choices update later suggestions |
| 2. Audience control | Lifecycle filter (five groups, expandable to ten stages) and permission level, with counts; reach options with counts; a suggested "Add lookalike readers" option; a description of the next readers added | M1: filters, options and counts. M2: lookalikes and the next-readers description |
| 3. Safety and value | Risk per channel (email complaint rate by provider, push disable rate, SMS opt-out rate, in-app dismiss rate); one-line send status (for example "Risk high · Microsoft"); ranges for reads, subscription starts, and unsubscribes or disables | M1: email risk and status. M2: other channels and value ranges |
| 4. Timing and send controls | Timing: Fixed (now or a set time), Geo-based (a set local time in each time zone, with a fallback when the zone is unknown), or Reader-based (each reader's usual reading time, within the content's relevance window). Plus frequency skip across channels; quiet hours; urgent-send allowance; overlap detection; staged waves for large sends; overrides with a reason code; sign-off when a limit is exceeded | M1: Fixed and Geo-based timing, frequency skip and overrides. M2: Reader-based timing, waves, overlap and allowance |
| 5. Message and channel | A "Send on" choice of Single channel, Multiple channels or Dynamic. Multiple channels shows a count of readers getting more than one message. Dynamic uses declared preference only, or declared then predicted, with allowed channels, a fallback and a channel-split preview. Content cards for each channel in use: email as the source, a suggested push draft from the email, in-app built automatically from the email, and SMS written by hand | M1: Single and Multiple channels, with email, in-app and a manually written push. M2: Dynamic on declared preference, plus the push draft. M3: Dynamic on predicted preference |
| 6. Decision log and results | A log line saved for each send; projected vs. actual results at 24 hours and 7 days | M1: decision log. M2: projected vs. actual and summaries |

### ML models

| Need | What the builder requires | Proposed starting point |
|---|---|---|
| Reader data | Features from Segment events, with clicks rather than opens; engagement by channel and by mailbox provider; lifecycle stage, permissions and stated channel preference; a freshness timestamp on every score | Daily refresh, faster for breaking news |
| Reader scores | Per reader: likelihood to engage by channel; interest by topic, declared or inferred; likelihood to subscribe, stay or return, depending on the goal; complaint, unsubscribe, disable and opt-out risk by channel and provider | Separate models, or one model with several outputs |
| Ranking | Readers ranked by the value a send adds, minus a cost for risk, with the weighting set by the goal. A reach curve that sets the suggested reach and describes the next readers added | Uplift modeling, ranked against SQL audiences in a holdout test |
| Forecasts | Ranges for reads, subscription starts and opt-outs; complaint rate by provider | Ranges that capture actual results most of the time |
| Channel and time | One best channel and one usual reading time for each reader, inside permissions and quiet hours; a time zone for each reader for Geo-based timing | Recommendations first; automatic assignment, tested with holdouts, later |
| Next-readers description | A short description of the readers each larger reach option would add | Summaries derived from score drivers |
| Decision service | One API: goal, emphasis and limits in; suggestions, counts, risk and status out. Hard limits are enforced by rules the model cannot override | Suggestions update about 2 seconds after a change |
| Learning and monitoring | A standing holdout; projected vs. actual; overrides fed back into training; drift alerts; a fallback to rules-based audiences | Retrain on a schedule the ML team sets |

Questions for the ML team:

- Which models are worth building first, and which existing Segment Predictions outputs could be reused ([Twilio Segment](https://www.twilio.com/docs/segment/unify/traits/predictions))?
- What forecast accuracy and response time are realistic, and what data gaps block them?
- Which topics must never be inferred, such as health or politics?
- How should editorial priority interact with model ranking?

### AI layer

Three AI capabilities, all advisory. People keep the choice of story and emphasis, and every change to who receives a send.

| Capability | In the builder | Limit |
|---|---|---|
| Lookalike recommendations | "Add lookalike readers" is suggested in the Audience step. It shows how many readers it adds, people who read like the core audience but are not yet in it | Only readers with permission on an allowed channel; can be cleared in one click |
| Push draft from email | The Push card comes pre-filled with a title under 50 characters and one supporting line, written from the email's subject line and lead story | Never sent until a person chooses "Use draft" or edits it |
| Pre-send check | One line on Review that flags overlapping sends, use of the urgent allowance, permission gaps and spam triggers in the subject line | Advisory; a person decides whether to send |

Questions for the AI and ML teams:

- What source audience size and similarity cutoff make lookalikes reliable for news topics?
- Should lookalikes come from a warehouse model distributed through Segment, given Segment has none built in?
- Which pre-send checks should block a send, and which should only warn?
- What keeps push drafts accurate to the story and in the house voice, and how should their quality be measured?
- How do we measure whether lookalike readers engage as well as the core audience over time?

## Success measures

- Clicks per delivered message rise against the holdout group.
- Complaint rate stays under 0.08% at every provider, and push disable and SMS opt-out rates stay under their limits.
- Dynamic beats single channel on reads per reader against a holdout.
- Most push drafts are used as written or with light edits.
- Time from request to sendable audience falls from days to minutes.
- Most sends go out without warnings.
- Lookalike readers reach at least the core audience's click rate without raising complaint rates.

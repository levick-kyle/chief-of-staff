# Chief of staff for Claude

A setup file that turns Claude into a personal chief of staff. It reads what's coming at you across email, chat, meeting notes and your work tracker, ranks it against your actual goals, and hands you one short queue with a recommendation and a ready draft on each item. You approve, delegate, snooze, re-tier or kill. Every override gets logged, and every two weeks Claude interviews you and rewrites its own rubric from what you corrected.

The ranking is the point. Most of what's loudest in your inbox isn't what moves your goals, and a queue that learns your priorities from your overrides gets sharper every week.

**How to use it:** open a new Claude chat, attach this file, and send:

> Create a skill from the SKILL.md section of this file, then run it to set me up.

Setup takes about 20 minutes, mostly tappable questions.

**You'll need:**
- A paid Claude plan with scheduled tasks and artifacts (claude.ai web, desktop or mobile).
- Your tools connected in Claude. Use whatever you have:
  - Email: Gmail or Outlook / Microsoft 365
  - Chat: Slack or Microsoft Teams
  - Calendar: Google Calendar or Outlook
  - Meeting notes (optional): Granola, Otter, Fireflies, Fathom, Zoom, or Teams transcripts
  - Work tracker or CRM (optional): Linear, Jira, Asana, GitHub, Notion, HubSpot, Salesforce
- Any one source works. More sources make a better queue.

Everything it creates is private to you. Nothing ever sends: every outbound message is a draft you review and send yourself.

---

## SKILL.md

```
---
name: chief-of-staff-setup
description: Sets up a personal chief of staff: interviews the person, writes their own priorities skill, publishes their private queue page, and schedules their sweeps and biweekly check. Works with whatever email, chat, calendar, meeting-notes and tracker tools they have connected.
---
```

# Chief of staff setup

The person ends up with four things, all private to them:

1. **A priorities skill** ("<firstname>-priorities"): their goals, four tiers, routing rules and standing rules, in their words.
2. **A queue page:** one ranked list on phone and laptop. Each item has a recommendation and often a draft. They approve, delegate, snooze, re-tier, kill, or tap Tell Claude to correct an item. A quick-add box drops new items in, a Waiting tab tracks what others owe them, and an inbox sweep proposes filing for email they don't need to act on.
3. **A chief of staff scheduled task:** sweeps their tools, adds and re-ranks items, answers their Tell Claude notes, drafts chases, and turns approvals into drafts.
4. **A biweekly check:** every two weeks, asks them questions and tunes the skill from their overrides and standing rules.

## Privacy rules (read first)

- Build only from what the person tells you in this session and from their own connected tools. Never read anyone else's memory, chats, docs or pages.
- Everything you create is private to them: the skill, the page (artifacts start private), the scheduled tasks.
- Never write performance notes, comp, fundraising details, manager feedback or anything about a colleague's performance into any file.
- Growth goals are optional. Ask before writing them anywhere, and keep them only in their own skill.
- Ask permission before scanning their email, chat or calendar to pre-fill answers.
- Nothing ever sends. Every outbound message is a draft they review and send.

## Step 0: map their tools

Tools you need: scheduled tasks (create_trigger, list_triggers), artifact publishing (publish_files or publish_version, declare_capabilities, db_write, db_get), propose_skills, AskUserQuestion. If scheduled tasks or artifacts are missing, tell them their plan or surface doesn't support this setup and stop.

Then build a **source map**: for each of these jobs, which connected tool does it and which of its tools you'll call.

| Job | Common tools |
| --- | --- |
| Email: read, draft, label/file | Gmail, Outlook / Microsoft 365 |
| Chat: read, draft or DM | Slack, Microsoft Teams |
| Calendar: read, find free time | Google Calendar, Outlook |
| Meeting notes: read transcripts and action items | Granola, Otter, Fireflies, Fathom, Zoom, Teams |
| Work tracker: read assigned and blocking work | Linear, Jira, Asana, GitHub, Notion, a CRM |

Check what's connected and what each connector's tools can actually do (some can read but not draft). For any job with nothing connected, offer to connect a tool (SearchMcpRegistry + SuggestConnectors), then carry on with what's there. Tell them in one line what's covered and what isn't. Keep the source map: it goes into their skill and the task prompt.

Note two facts for later:
- **Instant drafts from the page** only work with the Gmail and Slack connectors, because the page calls their draft tools directly. With any other email or chat tool, Approve queues the item and the next scheduled run creates the draft.
- If their email or chat tool can't create drafts at all, the run leaves the finished text on the item for them to copy.

## Step 1: interview

Ask with AskUserQuestion, 4 questions per round, each standalone, recommended default first. They're often on mobile: tappable options, light text. Use plain-text questions only where the answer has to be their own words (goals). Offer once: "Want me to scan your last two weeks of email, chat and calendar to pre-fill the tier lists? I'll show you before saving anything." If yes, propose work types and VIPs from what you find and have them confirm.

**Round 1: role and north star**
- Role, team and company (free text, one line)
- Who they report to (or "nobody" for founders)
- The company goal their work ladders to, in their words (free text). Don't suggest one.
- Their own 1-3 outcomes this quarter (free text)

**Round 2: tiers.** Map their main work types to tiers. Start from the role defaults below and let them move items.
- **1A: core outcome.** Work that most directly moves their top outcome. Top of queue; Claude preps, they decide.
- **1B: strategic.** Decisions they frame for their manager, board or company, plus growth work if they opt in. Top of queue and protected time.
- **2: keep things running.** Necessary, batched on set days.
- **3: noise.** Weekly list, delegated or killed.
- Ask them to name tiers 1A and 1B in their own short words (used as labels on the page).

| Role | Usual 1A | Usual 2 |
| --- | --- | --- |
| Founder / CEO | Customers, fundraising, key hires | Team asks, vendors, admin |
| Ops | Delivery risks, escalations, launches | Day-to-day vendor and partner work, scheduling |
| Sales / BD / partnerships | Deals closing, pilots launching, qualified pipeline | Inbound screening, follow-ups, CRM hygiene |
| Engineering | Incidents, blocking reviews, work tied to a launch | Non-blocking reviews, cross-team asks |
| Product / design | Launch decisions, specs blocking eng | Feedback triage, research admin |
| Recruiting | Offers, final rounds, hiring-manager decisions | Scheduling, sourcing follow-ups |
| Marketing | Launches, campaigns tied to revenue | Content reviews, agency coordination |
| Finance | Close, cash, board reporting | Approvals, vendor payments |
| Customer success | At-risk accounts, renewals, escalations | Routine tickets, check-ins |
| Research | Milestones, results others depend on | Data requests, reviews |

**Round 3: routing**
- Max tier-1 items per day (default 3)
- Whose messages always skip batching (multi-select, tailored to them: their manager, their directs, customers, investors or board, key partners)
- Events that always surface immediately (multi-select, tailored to role: production incident, legal or contract exposure, customer escalation, a deal or offer deadline, a payment failure, anything from a named VIP)
- When an unanswered thread gets promoted (default: 2nd chase)
- If they own payments or approvals: days past the ask before one surfaces (default 5)

**Round 4: delegation and rhythm**
- Who they can delegate to (free text names, then a one-line description of what each owns). If their chat tool can look people up, find each person's user id yourself and confirm the match; the page uses it to draft the DM on tap.
- Daily Top 3 time (default 7:00am local)
- Sweeps per day: 3 (morning, midday, evening), 2, or 1. Each run uses their Claude plan.
- Batch days for tier 2 (default Tue + Fri)

**Round 5: lanes and notifications**
- Push notifications from the chief of staff: none, since the page shows everything (recommended), tripwires + VIPs only, or all tier 1
- Chase lane: track what others owe them and draft nudges when they're due? Who it may nudge: vendors, partners and teammates (recommended), or also customers
- Inbox sweep: propose filing for email they don't need to act on, which they approve from the page? (yes / no). If yes, read their labels or folders and confirm which one each kind of sender goes to.
- Anything the agent should never queue (free text, optional; e.g. receipts their EA handles)
- Names or topics that must never appear in drafts to certain audiences (free text, optional; e.g. a confidential customer never named to vendors)

**Round 6: optional extras**
- Two weekly strategy blocks picked from their calendar each Sunday? (yes / no)
- Friday time audit (hours by tier vs a budget)? If yes, budget split (default 55 / 30 / 15)
- Friday manager update: written draft, 5-minute talk track, both, or none
- Private growth ledger: log each recommendation they bring their manager and whether it needed reframing? Off by default. Skip if they don't report to anyone.

Ask one more round only if an answer left something essential open. Keep the interview under 25 questions.

## Step 2: write their priorities skill

Fill this template from their answers and call propose_skills (kind new, name "<firstname>-priorities", description "<Name>'s priority rubric for ranking, routing and drafting their work. Load before triaging their email, chat or meeting notes, building their queue or Top 3, deciding what to delegate, or running their biweekly check."). Leave out any section they opted out of. Write plainly, with contractions and no em dashes.

```
# <Name> priorities

<Name> is <role> at <company>. Every item that reaches them gets one tier and one routing decision first. Fewer, better items beat complete lists.

## North star
- Company: <company goal>
- <Name>: <their 1-3 outcomes>

## Tiers
**1A: <their label>.** Top of queue. Prep the decision; <Name> makes it.
- <work types>
**1B: <their label>.** Top of queue + protected time. Draft structure, never their position.
- <work types>
**2: Keep things running.** Batched <days>, drafted send-ready.
- <work types>
**3: Noise.** Weekly list only.
- <work types, who screens>

## Routing rules
- Max <N> tier-1 items a day. If more appear, ask which one drops.
- Split decision from execution. Decisions go to <Name>; execution goes to an owner below with a drafted brief.
- Real deadlines only. Never manufacture urgency.
- Never queue: <their list, if any>.

**Always immediate (skip the batch):**
- <VIPs>
- <tripwires and thresholds>

## Sources
- <job>: <tool> (<what it can do: read / draft / label>)
- <jobs with no tool: say so>

## Who they delegate to
- <names + what each owns>
Delegation = a chat DM draft to that person for one-tap approval. Never sent without approval.

## Who's who
- <names + one-line roles for the people who show up most, confirmed with them. Roles only.>

## Chase lane (only if on)
Track concrete asks <Name> made with a named owner and a deliverable. Nudge <who it may nudge>. Cadence: a date in the thread wins; otherwise 3 business days for vendors, partners and customers, 2 for teammates and their manager. Chases are very short, no rehash, a reply in the original thread. For their manager, raise it as a 1B item for the next 1:1 instead of a nudge. After two unanswered nudges, recommend a call, a loop-in or dropping it.

## Inbox sweep (only if on)
- <sender kind> → <their label or folder>
Archive with no label: newsletters, answered calendar invites, receipts nobody acts on. Never touch promotional or social categories. Never delete, mark spam or unsubscribe.

## Rhythm
- Top 3 at <time>, one line each on why it matters, plus the tier-2 batch count.
- Tier-2 batches <days>.
- Push notifications: <choice>.
- <strategy blocks / Friday time audit / Friday update, if chosen>
- 1st and 15th: biweekly check.

## Growth ledger (only if opted in)
<their bar>. After each interaction with their manager, pre-fill one entry per recommendation: question + date, their position, whether it was reframed, outcome. They confirm.

## Queue item format
Tier, one line on why it matters, recommendation, draft (email / chat / doc edit) with its machine target, source links, est. minutes. Actions: Approve, Tell Claude, Delegate, Snooze, Re-tier, Kill.

## Decisions log and standing rules
The page logs every override (re-tier, kill, edit, delegate, Tell Claude note) in decisions/<date>. Standing instructions <Name> gives through Tell Claude ("stop queuing X", "Y is always tier 2") live in meta/rules and override this skill until the biweekly check folds them in.

## Biweekly check
Every two weeks, in a fresh session: gather the last 14 days (decisions log, meta/rules, queue and chase history, recent chats, chat and meeting notes), find what's stale or wrong, ask up to 15 tappable questions (findings first, 4 per round, default first), then propose edits to this skill as edits. Fold adopted meta/rules into the skill and clear them from meta/rules.

## Standing rules
- Approve in the queue = create a draft for that one item. Never send.
- No browser automation without asking.
- No new files in folders shared outside the company without review.
- Never name: <their list, and to whom>.
- Email drafts use their signature, so don't add a name under the sign-off.
- Chat drafts: plain short paragraphs, no bullet characters.
- Write plainly: contractions, no em dashes, no filler.
- <any of their own>
```

## Step 3: publish their queue page

1. Write the page in the appendix to a file, exactly as given.
2. Publish it as a new artifact: title "<Name>'s Queue", icon "inbox". Keep the slug and url.
3. Declare capabilities (publish first, then declare). Always declare the database and the run-now server. Add Gmail and Slack only if they use those connectors, with the server names their connectors show:
   ```
   {"db": {"rules": [{"path": "", "read": "admin", "write": "owner"}]},
    "mcp": {"servers": [
      {"server": "Claude Code Remote", "tools": ["fire_trigger"]},
      {"server": "<Gmail server name>", "tools": ["create_draft", "label_thread", "unlabel_thread"]},
      {"server": "<Slack server name>", "tools": ["slack_send_message_draft"]}]},
    "user": {}}
   ```
   Claude Code Remote powers Run sweep now and Tell Claude. Gmail and Slack let Approve, Delegate and chases create drafts on the spot and let the inbox sweep apply from the page. If the mcp part is refused, declare db and user only: the page still works, and approvals wait for the next sweep.
4. db_write two documents:
   - `meta/config`: `{"name": "<first name>", "managerName": "<manager's first name>", "delegates": [{"name": "<name>", "chatId": "<chat user id, if found>"}], "tierNames": {"1A": "<label>", "1B": "<label>", "2": "Keep things running", "3": "Noise"}, "batchDays": [<0=Sun..6=Sat, default 2, 5>], "batchLabel": "<e.g. Tue + Fri>", "emailApp": "<e.g. Outlook>", "chatApp": "<e.g. Teams>", "gmailServer": "<Gmail server name, only if Gmail>", "slackServer": "<Slack server name, only if Slack>"}`. Leave out any field that doesn't apply. The trigger id goes in after Step 4.
   - `meta/today`: `{"top3": [], "note": "Set up <date>. First sweep runs <time>."}`
5. Optional, with their OK: run a first sweep now and seed 5-15 real items so the page isn't empty tomorrow.

The page's database:
- `items/<id>`: title, tier "1A"/"1B"/"2"/"3", why, rec, draft {kind "email"/"chat"/"doc", to, subject, body, plus machine targets: toEmails [..], cc [..], replyToMessageId for email; channelId, threadTs for chat}, sources [{label,url}], minutes, rank, trip, status "open"/"approved"/"delegated"/"snoozed"/"killed"/"done", delegateTo, snoozeUntil, note, edited, draftCreatedAt, draftedFrom, draftLink, createdAt, updatedAt, plus Tell Claude fields: thread [{from "owner"/"claude", text, at}], askPending, askedAt.
- `asks/<id>`: notes about the queue as a whole from the page's top Tell Claude box: text, status "new"/"answered", reply, createdAt, answeredAt, dismissed.
- `inbox/<id>`: quick adds from the page's Add to queue box: text, status "new"/"filed", createdAt, itemId.
- `waits/<id>`: the chase lane: who, whoKind "partner"/"vendor"/"customer"/"accounting"/"teammate"/"manager", ask, channel, sourceUrl, askedAt, dueAt, nextChaseAt, cadenceDays, chaseCount, lastChasedAt, status "waiting"/"received"/"dropped", draft (same shape as item drafts), chaseApproved, escalation, note.
- `sweeps/current`: the inbox sweep proposal: createdAt, updatedAt, status "proposed"/"approved"/"applied"/"skipped", threads [{threadId, subject, from, action "keep"/"label"/"file"/"archive", labelId, labelName, reason, url, applied}], unsubscribe [{sender, reason}].
- `meta/today` (top3 [ids], lastSweep, note), `meta/config`, `meta/rules` (rules [{text, addedAt, source}]), `meta/notesRun` (status "running"/"idle", startedAt, finishedAt).
- `decisions/<YYYY-MM-DD>` (the page appends to an "entries" array), `ledger/<id>` (growth ledger, if used).

## Step 4: schedule their tasks

Use create_trigger with initiation human_request. Cron in their time zone (CRON_TZ=<zone>). Move on-the-hour times 1-29 minutes earlier per the tool's jitter rule. Set notifications from their push answer.

**Chief of staff task.** Name "<Name> chief of staff". Schedule from their sweeps-per-day answer, with the morning run ending before their Top 3 time. Then read meta/config (db_get) and write it back with `"triggerId": "<the new trig_ id>"` added (pass ifVersion). That turns on Run sweep now and Tell Claude on the page. Prompt, filled in (drop the bracketed parts they didn't choose):

```
You're <Name>'s chief of staff agent. <Name> is <role> at <company>. Each run, keep their queue page ranked, current and short: <page url> (artifact slug <slug>). Its database holds items/<id>, asks/<id>, inbox/<id>, waits/<id>, sweeps/current, meta/today, meta/config, meta/rules, meta/notesRun, decisions/<date> and ledger/<id>, with the fields listed in the chief-of-staff-setup skill. Item drafts carry machine targets: email drafts have toEmails, cc and replyToMessageId (when replying); chat drafts have channelId (a user id works for DMs) and threadTs (when replying in a thread).

Their tools: <source map: job, tool, which of its tools to call for reading, drafting and filing>.

The page <if Gmail or Slack: creates Gmail and Slack drafts itself when <Name> taps Approve, Delegate or Approve chase, applies the inbox sweep, and> can start this run on demand. Items it handled are already status "done" with draftCreatedAt set and draftedFrom "page": never draft those again. <Name> can tap Tell Claude on an item (the page adds the note to that item's thread and sets askPending true) or type in the top Tell Claude box (saved as asks/<id>, status "new"). Either one starts a notes-only run unless meta/notesRun says one is already running.

Hard rule: never send an email or chat message. Every outbound message is a draft <Name> reviews and sends.

NOTES-ONLY RUNS. If this run's extra message starts with "Tell Claude (notes only)", do only this, then stop:
a. Load <firstname>-priorities. Set meta/notesRun to {status "running", startedAt now}. Read meta/rules, meta/today, every item with askPending true, and every asks doc with status "new".
b. Answer each item note as step 3b says.
c. Answer each asks/<id>. These are about the queue as a whole. Check facts in their tools only when the note needs it. Then act: reorder or swap meta/today.top3 (tier 1 only) and update meta/today.note; re-tier, kill, snooze or retitle items; or, when the note is a standing instruction ("stop queuing X", "Y is always tier 2", "never chase Z"), append it to meta/rules.rules as {text in their words, addedAt, source "asks/<id>"} and apply it to the current queue. Log every re-tier, kill, Top 3 change and new rule in decisions/<today> with their note as the reason. Then update the ask: status "answered", reply (one to three plain sentences on what you found or changed, or one short question if you need something only they know), answeredAt now.
d. Set meta/notesRun to {status "idle", finishedAt now}. Re-read: if a note landed while you worked, set "running" again and repeat b through d.
e. Skip everything else: no sweep, no inbox or chase work, don't touch lastSweep, don't rewrite the Top 3 unless a note asked, no push notification. End with one line on what you changed.
Every other run (scheduled, or Run sweep now) is a full run: steps 1 through 12.

1. Load the <firstname>-priorities skill and follow it exactly. It's the rubric.
2. Read the queue database with db_list / db_query / db_get, including meta/rules. Follow every rule in meta/rules; a rule there overrides this prompt where they conflict, except the hard rules on sending and deleting.
3. Execute approvals the page couldn't draft instantly. Approval of a queue item is explicit approval for that one draft only. Skip anything with draftCreatedAt. approved + email draft = an email draft in their email tool, with their signature; approved + chat draft = a chat draft in their chat tool; approved + no draft = done; delegated = a chat DM draft to delegateTo (what's needed, by when, links). If their tool can't create drafts, leave the final text in the item's draft, set note "Copy this: <tool> can't draft", and set status "done". Never send. Then set status "done" and draftCreatedAt.
3b. Answer Tell Claude notes. For every item with askPending true, treat the latest note in its thread as their instruction for that item. Check the facts in their tools first. Do what it asks within these rules (correct, redraft, retitle, re-tier, kill, delegate, or turn it into a wait), then append your reply to thread as {from "claude", text, at}: one to three plain sentences on what you found or changed, answering any question directly, including whether they were right. If you need something only they know, ask one short question and leave the item open. Set askPending false. Log re-tiers, kills and reroutes in decisions/<today> with their note as the reason. Also answer asks docs with status "new" as notes-only step c says. A note never authorizes sending.
4. Process the inbox: turn every inbox/<id> with status "new" into a queue item (or update the matching one; keep any link as a source), then set the inbox doc to status "filed" with itemId. If the text is too thin to tier, make it tier 2 and say what's missing in the note.
5. Sweep since meta/today.lastSweep (first run: last 48 hours): <connected sources>. Only add items <Name> personally owes or must decide. No meetings, nothing others owe (those go to the chase lane).
6. For each new item: dedupe (same thread or ask = update the existing item), tier it, set trip if a tripwire fires, write a one-line imperative title, a one-line why naming the real forcing function, a concrete rec, minutes, source links, rank. Draft only when the content is clear from the source, and always include the machine target so Approve works. Check the calendars of everyone a scheduling draft names before proposing times. Kebab-case ids.
7. Re-check open items: set "done" with note "handled outside queue" when they already handled it. Refresh trips.
8. <If chase lane: Track what others owe <Name> in waits/<id>, per the skill's Chase lane section. Add a wait from their sent mail, chat and meeting notes when they made a concrete ask with a named owner and deliverable; skip vague asks. Dedupe by thread. nextChaseAt = askedAt or lastChasedAt + cadence, 9am local. Close a wait only when the thing arrived; an acknowledgment resets the clock. When a wait is due and has no draft, write one with machine targets. chaseApproved true: create that draft (unless the page already did), then chaseCount + 1, lastChasedAt now, nextChaseAt + cadence, draft null, chaseApproved false. Two unanswered chases and due again: set escalation to a one-line rec and write no draft. Never chase <who it may not nudge>.>
9. <If inbox sweep: Look only at their inbox, skipping promotional and social categories. Mark each thread keep (they owe something; make sure an item exists), label, file (label + archive) or archive, per the skill's Inbox sweep section, with label or folder ids from their email tool. Write sweeps/current with status "proposed"; replace it only when it isn't "proposed" or "approved", or is more than a day old. If sweeps/current is "approved", apply every thread whose action isn't "keep" with their email tool (label or move to folder, then archive), mark each applied, and set status "applied". Never file anything they haven't approved. Never delete, mark spam or unsubscribe.>
10. <If growth ledger: pre-fill ledger entries after manager interactions and add a 1B item to confirm them.>
11. Pick the Top 3 (tier 1 only; real deadlines first) and write meta/today: top3, lastSweep, a one-line note. If open tier-1 items exceed 9, propose demotions in the note instead of adding more. <Sunday evening run: propose strategy blocks as a 1B item. Friday evening run: time audit and manager update draft as 1B items. Only the ones they chose.>
12. End with a 3-line summary: Top 3, new tier-1 items, tripwires (including escalated waits).

Rules: one db_write call at a time; read first and pass ifVersion for existing documents; at most 50 ops per call. Never send email or chat messages, never delete mail, never use a browser, never put files in folders shared outside the company. Never name <their never-name list> to <audience>. Write plainly, with contractions and no em dashes.
```

**Biweekly check task.** Name "<Name> biweekly check". Cron: 1st and 15th, early evening. Prompt, filled in:

```
You're running <Name>'s biweekly priorities check. The goal is to refine their <firstname>-priorities skill so their agent needs fewer corrections.

1. Load <firstname>-priorities and follow its Biweekly check section.
2. Gather the last 14 days from their queue page (slug <slug>): the decisions log, meta/rules, items, Tell Claude threads and the chase lane. Add recent chats, chat messages and meeting notes.
3. Find what's stale or wrong: override patterns, standing rules worth making permanent, new projects or people the tiers don't name, ownership changes, tripwires that fired too often or missed, chases that went to the wrong people.
4. Send one short message with the 2-3 biggest findings, then ask up to 15 questions with AskUserQuestion, 4 per call, default first, light text. If they aren't there, leave the questions and wait.
5. From their answers, propose edits with propose_skills (kind improvement, target <firstname>-priorities, as edits). Once they save them, remove the folded-in rules from meta/rules.

Never send email or chat messages and never use a browser. Write plainly, with contractions and no em dashes.
```

In your reply, say in one sentence which approval setting each task got. If its runs will ask before acting, mention they can switch it to automatic approval in the task's settings.

## Step 5: hand off

One short message:
- The queue page link and what's on it
- When the sweeps run and when the first biweekly check is
- How to use it: tap a card, then Approve (creates the draft, never sends), Tell Claude, Delegate, Snooze, Re-tier or Kill. Add to queue drops in a new item; the top Tell Claude box takes feedback on the Top 3 or a standing rule; Run sweep now refreshes on demand; Waiting shows what others owe them.
- Whether approvals draft instantly (Gmail, Slack) or on the next run (everything else)
- The first time they approve a draft or run a sweep, claude.ai asks once to allow that connector for the page. Allow it.
- Scheduled run chats are receipts: don't reply in them. Corrections go through Tell Claude on the page, and deep work on an item gets its own chat.
- Their skill card is above; save it before the first sweep
- How to pause: turn off the tasks in their scheduled tasks list

## Appendix: queue page

Publish exactly this. It reads its name, manager, delegates, tier labels, batch days, app names, instant-draft connectors and trigger id from `meta/config`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Queue</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Manrope:wght@400;500;600;700&display=swap" rel="stylesheet">
<style>
  /* Layout: one narrow column of decision cards, tabs by tier, actions inline on the open card */
  :root {
    --bg: #f6f5f2; --card: #ffffff; --ink: #1c1b19; --muted: #6b6862; --line: #e4e1da;
    --accent: #2f5bd3; --accent-soft: #e8eefc; --b1: #7a4cc9; --b1-soft: #f1ebfb;
    --t2: #5d6b5a; --t2-soft: #edf1ec; --t3: #8a8780; --t3-soft: #f0efec;
    --warn: #b4531a; --warn-soft: #fbeee5; --good: #2f7d4f; --good-soft: #e6f2ea; --danger: #b3322b;
    --on-accent: #ffffff;
    --font: Manrope, system-ui, -apple-system, "Segoe UI", sans-serif;
  }
  @media (prefers-color-scheme: dark) {
    :root:not([data-theme="light"]) {
      --bg: #141413; --card: #1e1d1b; --ink: #ecebe7; --muted: #9b988f; --line: #2f2d2a;
      --accent: #7c9cf2; --accent-soft: #1e2740; --b1: #b294ec; --b1-soft: #2a2238;
      --t2: #9fb09b; --t2-soft: #222a21; --t3: #8f8c85; --t3-soft: #252422;
      --warn: #e59563; --warn-soft: #33241a; --good: #6cc391; --good-soft: #1d2b22; --danger: #ec7a72;
      --on-accent: #10131c; color-scheme: dark;
    }
  }
  :root[data-theme="dark"] {
    --bg: #141413; --card: #1e1d1b; --ink: #ecebe7; --muted: #9b988f; --line: #2f2d2a;
    --accent: #7c9cf2; --accent-soft: #1e2740; --b1: #b294ec; --b1-soft: #2a2238;
    --t2: #9fb09b; --t2-soft: #222a21; --t3: #8f8c85; --t3-soft: #252422;
    --warn: #e59563; --warn-soft: #33241a; --good: #6cc391; --good-soft: #1d2b22; --danger: #ec7a72;
    --on-accent: #10131c; color-scheme: dark;
  }
  * { box-sizing: border-box; }
  body { margin: 0; background: var(--bg); color: var(--ink); font: 15px/1.45 var(--font); -webkit-font-smoothing: antialiased; }
  .wrap { max-width: 760px; margin: 0 auto; padding: 20px 16px 80px; }
  header.top { display: flex; align-items: center; justify-content: space-between; gap: 12px; flex-wrap: wrap; }
  .top-right { display: flex; align-items: center; gap: 10px; flex-wrap: wrap; }
  h1 { font-size: 22px; margin: 0; font-weight: 700; letter-spacing: -0.01em; }
  .sweep { color: var(--muted); font-size: 13px; }
  button.run { border: 1px solid var(--line); background: var(--card); color: var(--ink); border-radius: 999px; padding: 7px 14px; font: 600 13px var(--font); cursor: pointer; min-height: 36px; }
  button.run:disabled { opacity: 0.55; cursor: default; }
  nav { display: flex; gap: 6px; margin: 16px 0 12px; overflow-x: auto; }
  nav button { border: 1px solid var(--line); background: var(--card); color: var(--ink); border-radius: 999px; padding: 7px 14px; font: 600 13px var(--font); cursor: pointer; white-space: nowrap; }
  nav button.on { background: var(--ink); color: var(--bg); border-color: var(--ink); }
  nav .n { opacity: 0.6; margin-left: 4px; font-weight: 500; }
  h2 { font-size: 13px; text-transform: uppercase; letter-spacing: 0.06em; color: var(--muted); margin: 22px 0 8px; font-weight: 700; }
  .note-bar { font-size: 13px; color: var(--muted); margin: 4px 0 8px; }
  .card { background: var(--card); border: 1px solid var(--line); border-radius: 12px; margin: 8px 0; overflow: hidden; }
  .card.sel { outline: 2px solid var(--accent); outline-offset: -1px; }
  .card .head { display: flex; gap: 10px; align-items: flex-start; padding: 12px 14px; cursor: pointer; }
  .num { font-weight: 700; color: var(--accent); min-width: 14px; }
  .chip { font-size: 11px; font-weight: 700; padding: 2px 7px; border-radius: 6px; white-space: nowrap; margin-top: 2px; }
  .c1A { background: var(--accent-soft); color: var(--accent); }
  .c1B { background: var(--b1-soft); color: var(--b1); }
  .c2 { background: var(--t2-soft); color: var(--t2); }
  .c3 { background: var(--t3-soft); color: var(--t3); }
  .ttl { flex: 1; min-width: 0; }
  .ttl h3 { margin: 0; font-size: 15px; font-weight: 600; }
  .ttl .why { margin: 2px 0 0; color: var(--muted); font-size: 13px; }
  .trip { color: var(--warn); font-size: 12px; font-weight: 600; margin-top: 3px; }
  .min { color: var(--muted); font-size: 12px; white-space: nowrap; margin-top: 2px; }
  .body { padding: 0 14px 14px; border-top: 1px solid var(--line); }
  .lab { font-size: 11px; text-transform: uppercase; letter-spacing: 0.06em; color: var(--muted); font-weight: 700; margin: 12px 0 4px; }
  .rec { margin: 0; white-space: pre-wrap; }
  .where { font-size: 12px; color: var(--muted); margin: 2px 0 4px; }
  .where.live { color: var(--good); }
  input.f, textarea.f { width: 100%; border: 1px solid var(--line); background: var(--bg); color: var(--ink); border-radius: 8px; padding: 8px 10px; font: 14px/1.45 var(--font); margin: 3px 0; }
  textarea.f { min-height: 140px; resize: vertical; }
  .srcs a { display: inline-block; color: var(--accent); font-size: 13px; margin: 2px 12px 2px 0; text-decoration: none; }
  .acts { display: flex; flex-wrap: wrap; gap: 6px; margin-top: 12px; }
  .acts button, .pick button { border: 1px solid var(--line); background: var(--card); color: var(--ink); border-radius: 8px; padding: 9px 14px; font: 600 14px var(--font); cursor: pointer; min-height: 40px; }
  .acts button.primary { background: var(--accent); border-color: var(--accent); color: var(--on-accent); }
  .acts button.danger { color: var(--danger); }
  .acts button:disabled, .pick button:disabled { opacity: 0.55; cursor: default; }
  .pick { display: flex; flex-wrap: wrap; gap: 6px; margin-top: 8px; padding: 8px; background: var(--bg); border-radius: 8px; }
  .pick span { width: 100%; font-size: 12px; color: var(--muted); }
  .done-link { display: inline-block; margin-top: 8px; font-size: 13px; color: var(--accent); text-decoration: none; }
  .st { font-size: 12px; font-weight: 600; color: var(--muted); }
  .due { font-size: 12px; font-weight: 700; color: var(--warn); white-space: nowrap; margin-top: 2px; }
  .due-bar { display: block; width: 100%; text-align: left; border: 1px solid var(--line); background: var(--warn-soft); color: var(--warn); border-radius: 10px; padding: 10px 14px; font: 600 14px var(--font); cursor: pointer; margin: 8px 0 4px; }
  .srow { display: flex; gap: 10px; align-items: flex-start; padding: 8px 0; border-bottom: 1px solid var(--line); cursor: pointer; }
  .srow input { margin-top: 3px; accent-color: var(--accent); }
  .sbody, .krow { display: flex; flex-direction: column; min-width: 0; }
  .krow { padding: 6px 0; border-bottom: 1px solid var(--line); }
  .sv { font-size: 12px; font-weight: 700; color: var(--t2); }
  .ss { font-size: 14px; overflow-wrap: anywhere; }
  .sf { font-size: 12px; color: var(--muted); overflow-wrap: anywhere; }
  .ulist { border-top: 0; padding-top: 6px; }
  .empty, .msg { color: var(--muted); padding: 24px 4px; text-align: center; font-size: 14px; }
  .keys { color: var(--muted); font-size: 12px; margin-top: 28px; text-align: center; }
  @media (hover: none) { .keys { display: none; } }
  button:focus-visible, input:focus-visible, textarea:focus-visible { outline: 2px solid var(--accent); outline-offset: 2px; }
  .toast { position: fixed; left: 50%; bottom: calc(20px + env(safe-area-inset-bottom, 0px)); transform: translateX(-50%); background: var(--ink); color: var(--bg); padding: 10px 16px; border-radius: 10px; font-size: 14px; opacity: 0; transition: opacity .2s; pointer-events: none; max-width: calc(100% - 32px); text-align: center; }
  .toast.on { opacity: 1; }
  form.add { display: flex; gap: 6px; margin: 14px 0 0; }
  form.add textarea { flex: 1; min-width: 0; min-height: 40px; height: 40px; resize: vertical; border: 1px solid var(--line); background: var(--card); color: var(--ink); border-radius: 8px; padding: 9px 10px; font: 14px/1.4 var(--font); }
  form.add button { border: 1px solid var(--accent); background: var(--accent); color: var(--on-accent); border-radius: 8px; padding: 0 16px; font: 600 14px var(--font); cursor: pointer; min-height: 40px; }
  form.add button:disabled { opacity: 0.55; cursor: default; }
  .inbox { margin: 6px 0 0; }
  .inbox .hd { font-size: 12px; color: var(--muted); margin: 4px 0; }
  .inbox .row { display: flex; gap: 8px; align-items: flex-start; font-size: 13px; padding: 6px 10px; background: var(--card); border: 1px dashed var(--line); border-radius: 8px; margin: 4px 0; }
  .inbox .row span { flex: 1; min-width: 0; white-space: pre-wrap; overflow-wrap: anywhere; }
  .inbox .row button { border: none; background: none; color: var(--muted); font: 600 13px var(--font); cursor: pointer; padding: 0 4px; min-height: 24px; }
  .thread { margin: 2px 0 0; }
  .msgk, .msgc { font-size: 14px; padding: 8px 10px; border-radius: 8px; margin: 4px 0; white-space: pre-wrap; overflow-wrap: anywhere; }
  .msgk { background: var(--bg); }
  .msgc { background: var(--accent-soft); }
  .msgw { display: block; font-size: 11px; font-weight: 700; color: var(--muted); margin-bottom: 2px; }
  .pending { color: var(--accent); font-size: 12px; font-weight: 600; margin-top: 3px; }
  .pick textarea.f { min-height: 80px; }
  .pick button.primary { background: var(--accent); border-color: var(--accent); color: var(--on-accent); }
  form.ask { margin-top: 6px; }
  form.ask button { background: var(--card); color: var(--accent); }
  .asks { margin: 6px 0 0; }
  .asks .a { background: var(--card); border: 1px solid var(--line); border-radius: 8px; padding: 6px 10px; margin: 4px 0; position: relative; }
  .asks .a .msgk, .asks .a .msgc { margin: 4px 0; }
  .asks .a button.x { position: absolute; top: 4px; right: 6px; border: none; background: none; color: var(--muted); font: 600 13px var(--font); cursor: pointer; min-height: 24px; }
  @media (prefers-reduced-motion: reduce) { .toast { transition: none; } }
</style>
</head>
<body>

<div class="wrap">
  <header class="top">
    <h1>Queue</h1>
    <div class="top-right">
      <div class="sweep" id="sweep"></div>
      <button class="run" id="runNow" type="button" hidden>Run sweep now</button>
    </div>
  </header>
  <form class="add" id="addForm" autocomplete="off">
    <textarea id="addText" rows="1" placeholder="Add to queue: a task, decision or link" aria-label="Add to queue"></textarea>
    <button type="submit" id="addBtn">Add</button>
  </form>
  <div class="inbox" id="inbox"></div>
  <form class="add ask" id="askForm" autocomplete="off">
    <textarea id="askTextTop" rows="1" placeholder="Tell Claude: feedback on the Top 3, the queue, or a standing rule" aria-label="Tell Claude about the queue"></textarea>
    <button type="submit" id="askBtnTop">Send</button>
  </form>
  <div class="asks" id="asks"></div>
  <nav id="tabs"></nav>
  <main id="list"><div class="msg">Loading your queue…</div></main>
  <div class="keys">j / k move · enter open · a approve · d delegate · s snooze · x kill · 1-4 re-tier (1A, 1B, 2, 3)</div>
</div>
<div class="toast" id="toast" role="status" aria-live="polite"></div>

<script>
(function () {
  const TIERS = ['1A', '1B', '2', '3'];
  // Everything person-specific lives in meta/config (see the setup file):
  // name, managerName, delegates [{name, chatId}], tierNames, batchDays (0=Sun..6=Sat), batchLabel,
  // triggerId, emailApp / chatApp (display names), gmailServer / slackServer (connector names that
  // enable instant drafts; leave unset for other mail or chat tools and the scheduled run drafts instead).
  const DEFAULT_TIERS = { '1A': 'Top outcome', '1B': 'Strategic', '2': 'Keep things running', '3': 'Noise' };
  const REMOTE = 'Claude Code Remote';
  let config = {};
  const cfgStr = (k) => (typeof config[k] === 'string' && config[k]) ? config[k] : null;
  const tierName = (t) => (config.tierNames && config.tierNames[t]) || DEFAULT_TIERS[t];
  const ownerName = () => cfgStr('name') || 'the owner';
  const managerName = () => cfgStr('managerName') || 'your manager';
  const emailApp = () => cfgStr('emailApp') || 'email';
  const chatApp = () => cfgStr('chatApp') || 'chat';
  const gmailServer = () => cfgStr('gmailServer');
  const slackServer = () => cfgStr('slackServer');
  const isChat = (d) => d && (d.kind === 'chat' || d.kind === 'slack');
  function people() {
    const list = Array.isArray(config.delegates) ? config.delegates : [];
    const out = {};
    list.forEach((p) => { if (typeof p === 'string') out[p] = null; else if (p && typeof p.name === 'string') out[p.name] = p.chatId || p.slackId || null; });
    return out;
  }
  const hasPeople = () => Object.keys(people()).length > 0;
  const triggerId = () => cfgStr('triggerId');
  const batchDays = () => (Array.isArray(config.batchDays) && config.batchDays.length ? config.batchDays.map(Number) : [2, 5]);
  const RUN_COOLDOWN_MS = 6 * 60 * 1000;
  const VIEWS = [
    { id: 'today', label: 'Today' },
    { id: 'waiting', label: 'Waiting' },
    { id: 'batch', label: 'Batch' },
    { id: 'weekly', label: 'Weekly' },
    { id: 'handled', label: 'Handled' }
  ];
  const KIND_LABEL = { partner: 'Partner', vendor: 'Vendor', customer: 'Customer', teammate: 'Team', manager: 'Manager', accounting: 'Accounting' };
  const CADENCE = { partner: 3, vendor: 3, customer: 3, accounting: 3, teammate: 2, manager: 2 };
  let waits = [], sweep = null, sweepOff = new Set(), sweepBusy = false;
  let db = null, mcp = null, items = [], meta = {}, view = 'today', openId = null, sel = 0, pick = null;
  let loaded = false, pendingRender = false, busy = new Set(), logChain = Promise.resolve();
  let runAvailable = false, runBusy = false;
  let notesRun = {}, asks = [], asking = false;
  const NOTES_STALE_MS = 15 * 60 * 1000;
  const drafts = {};
  const askDrafts = {};

  const $ = (s) => document.querySelector(s);
  const esc = (s) => String(s == null ? '' : s).replace(/[&<>"']/g, (c) => ({ '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#39;' }[c]));
  const safeUrl = (u) => (typeof u === 'string' && /^https?:\/\//i.test(u)) ? u : null;
  const nowIso = () => new Date().toISOString();

  function toast(t, ms) { const el = $('#toast'); el.textContent = t; el.classList.add('on'); clearTimeout(toast._t); toast._t = setTimeout(() => el.classList.remove('on'), ms || 2400); }

  function isOpen(it) {
    if (!it.status || it.status === 'open') return true;
    if (it.status === 'snoozed' && it.snoozeUntil && new Date(it.snoozeUntil) <= new Date()) return true;
    return false;
  }
  function top3Index(it) { const t = Array.isArray(meta.top3) ? meta.top3 : []; const i = t.indexOf(it.id); return i < 0 ? 99 : i; }
  function byRank(a, b) {
    return (top3Index(a) - top3Index(b)) || ((a.trip ? 0 : 1) - (b.trip ? 0 : 1)) || ((a.rank ?? 50) - (b.rank ?? 50)) || String(a.createdAt || '').localeCompare(String(b.createdAt || ''));
  }
  function listFor(v) {
    const open = items.filter(isOpen);
    if (v === 'today') return open.filter((i) => i.tier === '1A' || i.tier === '1B' || i.trip).sort(byRank);
    if (v === 'batch') return open.filter((i) => i.tier === '2' && !i.trip).sort(byRank);
    if (v === 'weekly') return open.filter((i) => i.tier === '3').sort(byRank);
    return items.filter((i) => !isOpen(i)).sort((a, b) => String(b.updatedAt || '').localeCompare(String(a.updatedAt || ''))).slice(0, 60);
  }

  // ---- Draft targeting: can this draft be created right now from the page? ----
  function emailsIn(d) {
    if (Array.isArray(d.toEmails) && d.toEmails.length) return d.toEmails.filter((e) => typeof e === 'string' && e.includes('@'));
    const m = String(d.to || '').match(/[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}/gi);
    return m ? Array.from(new Set(m)) : [];
  }
  function instantTarget(d) {
    if (!d || !mcp) return null;
    if (isChat(d) && d.channelId && slackServer()) return { server: slackServer() };
    if (d.kind === 'email' && emailsIn(d).length && gmailServer()) return { server: gmailServer() };
    return null;
  }

  // ---- Waiting (chase lane) ----
  function addBizDays(from, n) {
    const x = new Date(from); let k = 0;
    while (k < n) { x.setDate(x.getDate() + 1); const g = x.getDay(); if (g !== 0 && g !== 6) k++; }
    x.setHours(9, 0, 0, 0);
    return x.toISOString();
  }
  function waitDue(w) { return !w.nextChaseAt || new Date(w.nextChaseAt) <= new Date(); }
  function openWaits() {
    return waits.filter((w) => w.status === 'waiting').sort((a, b) => String(a.nextChaseAt || '').localeCompare(String(b.nextChaseAt || '')));
  }
  function shortDate(iso) {
    if (!iso) return '';
    const d = new Date(iso);
    return isNaN(d) ? '' : d.toLocaleDateString([], { weekday: 'short', month: 'short', day: 'numeric' });
  }
  function waitCardHtml(w, idx) {
    const key = 'w:' + w.id;
    const open = key === openId;
    const due = waitDue(w);
    const count = Number(w.chaseCount || 0);
    const escalate = due && count >= 2;
    const chip = `<span class="chip ${w.whoKind === 'manager' ? 'c1B' : w.whoKind === 'teammate' ? 'c2' : 'c1A'}">${esc(KIND_LABEL[w.whoKind] || 'Wait')}</span>`;
    const status = due ? `<span class="due">Chase due</span>` : `<span class="min">Next ${esc(shortDate(w.nextChaseAt))}</span>`;
    const meta = `Asked ${esc(shortDate(w.askedAt))}${w.dueAt ? ' · promised ' + esc(shortDate(w.dueAt)) : ''} · ${count === 0 ? 'not chased yet' : count === 1 ? 'chased once' : 'chased ' + count + 'x'}`;
    const trip = escalate ? `<div class="trip">⚑ ${esc(w.escalation || 'Two nudges, no answer. Call, loop someone in, or drop it?')}</div>` : '';
    let body = '';
    if (open) {
      const d = drafts[key] || w.draft || null;
      let draft = '';
      if (d && d.body) {
        const live = instantTarget(d);
        draft = `<div class="lab">Chase draft · ${esc(d.kind || 'draft')}</div>
          <div class="where${live ? ' live' : ''}">${live ? `Approve creates this ${d.kind === 'email' ? esc(emailApp()) : esc(chatApp())} draft in the original thread. You send it.` : 'Approve queues this for the next sweep, which creates the draft.'}</div>
          <input class="f" id="to-${esc(key)}" data-f="to" placeholder="To" aria-label="To" value="${esc(d.to || '')}">
          ${d.kind === 'email' ? `<input class="f" id="subject-${esc(key)}" data-f="subject" placeholder="Subject" aria-label="Subject" value="${esc(d.subject || '')}">` : ''}
          <textarea class="f" id="body-${esc(key)}" data-f="body" aria-label="Chase draft">${esc(d.body || '')}</textarea>`;
      } else {
        draft = `<p class="rec">${w.whoKind === 'manager' ? `No chase draft for ${esc(managerName())}. Raise it in your next 1:1.` : 'No draft yet. The next sweep writes one when this is due.'}</p>`;
      }
      const src = safeUrl(w.sourceUrl) ? `<div class="srcs"><a href="${esc(w.sourceUrl)}" target="_blank" rel="noopener">Open the thread ↗</a></div>` : '';
      const note = `<div class="lab">Note (optional)</div><input class="f" id="note-${esc(key)}" data-f="note" placeholder="e.g. they said end of month" aria-label="Note" value="${esc(w.note || '')}">`;
      const isBusy = busy.has(key) ? ' disabled' : '';
      let acts = `<div class="acts">
        ${d && d.body ? `<button type="button" class="primary" data-act="wchase"${isBusy}>${instantTarget(d) ? 'Approve chase + draft now' : 'Approve chase'}</button>` : ''}
        <button type="button" data-act="wgot"${isBusy}>Got it</button>
        <button type="button" data-act="pick" data-kind="wsnooze"${isBusy}>Snooze</button>
        <button type="button" class="danger" data-act="wdrop"${isBusy}>Drop</button></div>`;
      if (pick && pick.id === key && pick.kind === 'wsnooze') acts += `<div class="pick"><button type="button" data-act="wsnooze" data-arg="2">2 days</button><button type="button" data-act="wsnooze" data-arg="5">1 week</button></div>`;
      body = `<div class="body">${draft}${src}${note}${acts}</div>`;
    }
    return `<article class="card ${idx === sel ? 'sel' : ''}" data-id="${esc(key)}" data-idx="${idx}">
      <div class="head" data-act="toggle">${chip}<div class="ttl"><h3>${esc(w.who || '')}: ${esc(w.ask || '')}</h3><p class="why">${meta}</p>${trip}</div>${status}</div>${body}</article>`;
  }

  async function waitAct(key, kind, arg) {
    const id = key.slice(2);
    const w = waits.find((x) => x.id === id);
    if (!w || !db || busy.has(key)) return;
    const card = document.querySelector(`.card[data-id="${CSS.escape(key)}"]`);
    const noteEl = card && card.querySelector('[data-f="note"]');
    const note = noteEl ? noteEl.value.trim() : '';
    const base = drafts[key] || w.draft || null;
    let d = base;
    if (base && card) { d = Object.assign({}, base); card.querySelectorAll('[data-f]').forEach((el) => { if (el.dataset.f !== 'note') d[el.dataset.f] = el.value; }); }
    const patch = { updatedAt: nowIso() };
    if (note) patch.note = note;
    let msg = '';
    busy.add(key); render();
    try {
      if (kind === 'wchase') {
        const target = instantTarget(d);
        if (target) {
          toast(`Creating the ${target.server} draft…`, 8000);
          const r = await createDraft(d);
          if (r.ok) {
            patch.chaseCount = Number(w.chaseCount || 0) + 1;
            patch.lastChasedAt = nowIso();
            patch.nextChaseAt = addBizDays(new Date(), Number(w.cadenceDays) || CADENCE[w.whoKind] || 3);
            patch.draft = null; patch.chaseApproved = false;
            if (r.link) patch.draftLink = r.link;
            msg = `${target.server} draft is ready in the thread. Review and send it there.`;
          } else if (r.queue) {
            patch.chaseApproved = true; patch.draft = d; msg = r.msg;
          } else {
            await db.doc('waits/' + id).update(patch);
            toast(r.msg, 5000);
            return;
          }
        } else {
          patch.chaseApproved = true; patch.draft = d;
          msg = 'Approved. The next sweep creates the draft.';
        }
      }
      if (kind === 'wgot') { patch.status = 'received'; patch.receivedAt = nowIso(); msg = 'Marked received'; }
      if (kind === 'wdrop') { patch.status = 'dropped'; msg = 'Dropped'; }
      if (kind === 'wsnooze') { patch.nextChaseAt = addBizDays(new Date(), Number(arg)); msg = `Next nudge ${shortDate(patch.nextChaseAt)}`; }
      await db.doc('waits/' + id).update(patch);
      delete drafts[key]; pick = null; openId = null;
      logDecision({ waitId: id, title: `${w.who}: ${w.ask}`, action: kind, arg: arg || null, note, at: patch.updatedAt });
      toast(msg, msg.length > 60 ? 4500 : 2400);
    } catch (e) {
      toast("Couldn't save. Try again.");
    } finally { busy.delete(key); render(); }
  }

  // ---- Inbox sweep ----
  function sweepRows() { return sweep && Array.isArray(sweep.threads) ? sweep.threads : []; }
  function sweepActionable() { return sweepRows().filter((t) => t.action && t.action !== 'keep' && !t.applied); }
  function sweepVerb(t) {
    if (t.action === 'archive') return 'Archive';
    if (t.action === 'file') return `File in ${t.labelName || 'label'} + archive`;
    if (t.action === 'label') return `Label ${t.labelName || ''} (stays in inbox)`;
    return '';
  }
  function sweepCardHtml() {
    const rows = sweepActionable();
    if (!sweep || sweep.status !== 'proposed' || !rows.length) return '';
    const open = openId === 'sweep';
    const on = rows.filter((t) => !sweepOff.has(t.threadId));
    let body = '';
    if (open) {
      const list = rows.map((t) => `<label class="srow"><input type="checkbox" id="sw-${esc(t.threadId)}" data-sweep="${esc(t.threadId)}" ${sweepOff.has(t.threadId) ? '' : 'checked'}>
        <span class="sbody"><span class="sv">${esc(sweepVerb(t))}</span><span class="ss">${esc(t.subject || '(no subject)')}</span><span class="sf">${esc(t.from || '')}${t.reason ? ' · ' + esc(t.reason) : ''}</span></span></label>`).join('');
      const keep = sweepRows().filter((t) => t.action === 'keep');
      const keepList = keep.length ? `<div class="lab">Staying in your inbox</div>` + keep.map((t) => `<div class="krow"><span class="ss">${esc(t.subject || '')}</span><span class="sf">${esc(t.reason || '')}</span></div>`).join('') : '';
      const gmailOk = !!mcp && !!gmailServer();
      body = `<div class="body"><div class="where">${gmailOk ? `Apply labels and archives these in ${esc(emailApp())} now. Nothing is deleted.` : `Approve and the next sweep files these in ${esc(emailApp())}. Nothing is deleted.`}</div>${list}${keepList}
        <div class="acts"><button type="button" class="primary" data-act="sweepapply"${sweepBusy || !on.length ? ' disabled' : ''}>${sweepBusy ? 'Saving…' : `${gmailOk ? 'Apply' : 'Approve'} ${on.length}`}</button><button type="button" data-act="sweepskip"${sweepBusy ? ' disabled' : ''}>Skip this sweep</button></div></div>`;
    }
    return `<article class="card" data-id="sweep"><div class="head" data-act="toggle"><span class="chip c2">Inbox</span><div class="ttl"><h3>Inbox sweep: ${rows.length} thread${rows.length === 1 ? '' : 's'} to file</h3><p class="why">Proposed ${esc(shortDate(sweep.createdAt))}. You don't owe anything on these.</p></div><span class="min">2m</span></div>${body}</article>`;
  }

  async function approveSweep() {
    if (!db || sweepBusy || !sweep) return;
    const rows = sweepActionable().filter((t) => !sweepOff.has(t.threadId));
    if (!rows.length) return;
    sweepBusy = true; render();
    const threads = sweepRows().map((t) => sweepOff.has(t.threadId) && t.action !== 'keep' ? Object.assign({}, t, { action: 'keep', reason: 'Unchecked by owner' }) : t);
    try {
      await db.doc('sweeps/current').update({ threads, status: 'approved', approvedAt: nowIso(), updatedAt: nowIso() });
      toast(`Approved ${rows.length}. The next sweep files them.`, 3500);
      openId = null;
    } catch (e) { toast("Couldn't save. Try again."); }
    sweepBusy = false; sweepOff = new Set(); render();
  }
  async function applySweep() {
    if (!(mcp && gmailServer())) { approveSweep(); return; }
    if (!db || sweepBusy || !sweep) return;
    const rows = sweepActionable().filter((t) => !sweepOff.has(t.threadId));
    if (!rows.length) return;
    sweepBusy = true; render();
    const results = {}; let ok = 0, failed = 0, stop = null;
    for (const t of rows) {
      try {
        if (t.labelId && (t.action === 'file' || t.action === 'label')) await mcp.callTool(gmailServer(), 'label_thread', { threadId: t.threadId, labelIds: [t.labelId] }, { cache: false });
        if (t.action === 'file' || t.action === 'archive') await mcp.callTool(gmailServer(), 'unlabel_thread', { threadId: t.threadId, labelIds: ['INBOX'] }, { cache: false });
        results[t.threadId] = 'applied'; ok++;
      } catch (e) {
        const x = explain(e, emailApp());
        results[t.threadId] = 'failed'; failed++;
        if (x.queue) { stop = x.msg.replace(' Queued for the next sweep instead.', ''); break; }
      }
    }
    const threads = sweepRows().map((t) => results[t.threadId] === 'applied' ? Object.assign({}, t, { applied: true }) : t);
    const remaining = threads.filter((t) => t.action && t.action !== 'keep' && !t.applied).length;
    try {
      await db.doc('sweeps/current').update({ threads, status: remaining ? 'proposed' : 'applied', appliedAt: nowIso(), updatedAt: nowIso() });
    } catch (e) {}
    sweepBusy = false; sweepOff = new Set();
    if (!remaining) openId = null;
    toast(stop ? stop : failed ? `Filed ${ok}. ${failed} didn't go through; they're still listed.` : `Filed ${ok} thread${ok === 1 ? '' : 's'}.`, 4500);
    render();
  }
  async function skipSweep() {
    if (!db || !sweep) return;
    try { await db.doc('sweeps/current').update({ status: 'skipped', updatedAt: nowIso() }); toast('Skipped. The next sweep proposes a fresh batch.'); }
    catch (e) { toast("Couldn't save. Try again."); }
    openId = null; render();
  }

  function renderTabs() {
    $('#tabs').innerHTML = VIEWS.map((v) => {
      const n = v.id === 'handled' ? '' : v.id === 'waiting' ? `<span class="n">${openWaits().filter(waitDue).length || ''}</span>` : `<span class="n">${listFor(v.id).length}</span>`;
      return `<button type="button" data-view="${v.id}" class="${v.id === view ? 'on' : ''}">${v.label}${n}</button>`;
    }).join('');
  }

  function draftFor(it) { return drafts[it.id] || it.draft || null; }

  function cardHtml(it, idx, number) {
    const open = it.id === openId;
    const chip = `<span class="chip c${esc(it.tier)}">${esc(it.tier)}</span>`;
    const trip = it.trip ? `<div class="trip">⚑ ${esc(it.trip)}</div>` : '';
    const min = it.minutes ? `<span class="min">${esc(it.minutes)}m</span>` : '';
    const num = number ? `<span class="num">${number}</span>` : '';
    const st = view === 'handled' ? `<span class="st">${esc(it.status)}${it.delegateTo ? ' → ' + esc(it.delegateTo) : ''}</span>` : min;
    const thread = Array.isArray(it.thread) ? it.thread : [];
    const last = thread[thread.length - 1];
    const recentReply = last && last.from === 'claude' && last.at && (Date.now() - new Date(last.at)) < 48 * 3600e3;
    const pend = it.askPending ? `<div class="pending">Claude is working on your note…</div>` : recentReply ? `<div class="pending">Claude replied</div>` : '';
    let body = '';
    if (open) {
      const d = draftFor(it);
      const convo = thread.length ? `<div class="lab">With Claude</div><div class="thread">${thread.map((m) => `<div class="${m.from === 'claude' ? 'msgc' : 'msgk'}"><span class="msgw">${m.from === 'claude' ? 'Claude' : 'You'}${m.at ? ' · ' + esc(shortDate(m.at)) : ''}</span>${esc(m.text || '')}</div>`).join('')}</div>` : '';
      const rec = convo + (it.rec ? `<div class="lab">Recommendation</div><p class="rec">${esc(it.rec)}</p>` : '');
      let draft = '';
      if (d) {
        const kind = esc(d.kind || 'draft');
        const live = instantTarget(d);
        const where = d.kind === 'doc' ? '' : live
          ? `<div class="where live">Approve creates this ${d.kind === 'email' ? esc(emailApp()) : esc(chatApp())} draft now. Nothing is sent.</div>`
          : `<div class="where">Approve queues this for the next sweep, which creates the draft.</div>`;
        draft = `<div class="lab">Draft · ${kind}</div>${where}
          <input class="f" id="to-${esc(it.id)}" data-f="to" placeholder="To" aria-label="To" value="${esc(d.to || '')}">
          ${d.kind === 'email' ? `<input class="f" id="subject-${esc(it.id)}" data-f="subject" placeholder="Subject" aria-label="Subject" value="${esc(d.subject || '')}">` : ''}
          <textarea class="f" id="body-${esc(it.id)}" data-f="body" aria-label="Draft body">${esc(d.body || '')}</textarea>`;
      }
      const srcs = Array.isArray(it.sources) && it.sources.length
        ? `<div class="lab">Sources</div><div class="srcs">${it.sources.map((s) => { const u = safeUrl(s && s.url); return u ? `<a href="${esc(u)}" target="_blank" rel="noopener">${esc(s.label || 'link')} ↗</a>` : ''; }).join('')}</div>` : '';
      const note = `<div class="lab">Why (optional, trains the rubric)</div><input class="f" id="note-${esc(it.id)}" data-f="note" placeholder="e.g. not mine, belongs to someone else" aria-label="Why" value="${esc(it.note || '')}">`;
      const link = safeUrl(it.draftLink) ? `<a class="done-link" href="${esc(it.draftLink)}" target="_blank" rel="noopener">Open the draft ↗</a>` : '';
      const isBusy = busy.has(it.id) ? ' disabled' : '';
      let acts;
      if (view === 'handled') {
        acts = `${link}<div class="acts"><button type="button" data-act="reopen"${isBusy}>Reopen</button></div>`;
      } else {
        const approveLabel = d && d.kind !== 'doc' ? (instantTarget(d) ? 'Approve + draft now' : 'Approve draft') : 'Done';
        acts = `<div class="acts">
          <button type="button" class="primary" data-act="approve"${isBusy}>${approveLabel}</button>
          <button type="button" data-act="pick" data-kind="ask"${isBusy}>Tell Claude</button>
          ${hasPeople() ? `<button type="button" data-act="pick" data-kind="delegate"${isBusy}>Delegate</button>` : ''}
          <button type="button" data-act="pick" data-kind="snooze"${isBusy}>Snooze</button>
          <button type="button" data-act="pick" data-kind="retier"${isBusy}>Re-tier</button>
          <button type="button" class="danger" data-act="kill"${isBusy}>Kill</button></div>`;
        if (pick && pick.id === it.id) {
          if (pick.kind === 'ask') acts += `<div class="pick"><span>${runAvailable && triggerId() ? 'Sends this to Claude now. It checks what it needs, then answers here, redrafts or re-tiers. No full sweep. Usually 1-3 minutes.' : 'Saved for the next sweep, which answers here.'}</span><textarea class="f" id="ask-${esc(it.id)}" aria-label="Note for Claude" placeholder="e.g. didn't they already send this? Or: make this shorter">${esc(askDrafts[it.id] || '')}</textarea><button type="button" class="primary" data-act="ask"${isBusy}>Send to Claude</button></div>`;
          if (pick.kind === 'delegate') acts += `<div class="pick"><span>${mcp && slackServer() ? `Creates a ${esc(chatApp())} DM draft now for you to review and send` : `Queues a ${esc(chatApp())} DM draft for the next sweep`}</span>${Object.keys(people()).map((p) => `<button type="button" data-act="delegate" data-arg="${esc(p)}"${isBusy}>${esc(p)}</button>`).join('')}</div>`;
          if (pick.kind === 'snooze') acts += `<div class="pick"><button type="button" data-act="snooze" data-arg="1">Tomorrow</button><button type="button" data-act="snooze" data-arg="3">3 days</button><button type="button" data-act="snooze" data-arg="batch">Next batch</button></div>`;
          if (pick.kind === 'retier') acts += `<div class="pick">${TIERS.filter((t) => t !== it.tier).map((t) => `<button type="button" data-act="retier" data-arg="${t}">${t} · ${esc(tierName(t))}</button>`).join('')}</div>`;
        }
      }
      body = `<div class="body">${rec}${draft}${srcs}${note}${acts}</div>`;
    }
    return `<article class="card ${idx === sel ? 'sel' : ''}" data-id="${esc(it.id)}" data-idx="${idx}">
      <div class="head" data-act="toggle">${num}${chip}<div class="ttl"><h3>${esc(it.title)}</h3>${it.why ? `<p class="why">${esc(it.why)}</p>` : ''}${trip}${pend}</div>${st}</div>${body}</article>`;
  }

  function render() {
    const ae = document.activeElement;
    if (ae && ae.closest && ae.closest('.card') && /INPUT|TEXTAREA/.test(ae.tagName)) { pendingRender = true; return; }
    pendingRender = false;
    renderTabs();
    $('#sweep').textContent = meta.lastSweep ? `Last sweep ${new Date(meta.lastSweep).toLocaleString([], { weekday: 'short', hour: 'numeric', minute: '2-digit' })}` : '';
    renderRun();
    const list = view === 'waiting' ? openWaits() : listFor(view);
    if (sel >= list.length) sel = Math.max(0, list.length - 1);
    let html = '';
    if (view === 'waiting') {
      const due = list.filter(waitDue), later = list.filter((w) => !waitDue(w));
      let idx = 0;
      html = `<div class="note-bar">What others owe you. Every chase is a draft you send yourself.</div>`;
      if (due.length) html += `<h2>Chase due</h2>` + due.map((w) => waitCardHtml(w, idx++)).join('');
      if (later.length) html += `<h2>Waiting</h2>` + later.map((w) => waitCardHtml(w, idx++)).join('');
      if (!list.length) html += `<div class="empty">Nobody owes you anything right now. New waits show up here from your sent mail, chat and meetings.</div>`;
      $('#list').innerHTML = html;
      return;
    }
    if (view === 'today') {
      const top = list.filter((i) => top3Index(i) < 99);
      const trips = list.filter((i) => top3Index(i) === 99 && i.trip);
      const rest = list.filter((i) => top3Index(i) === 99 && !i.trip);
      let idx = 0;
      if (meta.note) html += `<div class="note-bar">${esc(meta.note)}</div>`;
      const dueN = openWaits().filter(waitDue).length;
      if (dueN) html += `<button type="button" class="due-bar" data-view="waiting">${dueN} chase${dueN === 1 ? '' : 's'} due · open Waiting →</button>`;
      if (top.length) html += `<h2>Top 3</h2>` + top.map((it, n) => cardHtml(it, idx++, n + 1)).join('');
      if (trips.length) html += `<h2>Tripwires</h2>` + trips.map((it) => cardHtml(it, idx++)).join('');
      if (rest.length) html += `<h2>Also tier 1</h2>` + rest.map((it) => cardHtml(it, idx++)).join('');
      if (!list.length) html = `<div class="empty">Nothing in tier 1. The chief of staff fills this on its next sweep.</div>`;
    } else if (view === 'batch') {
      const sw = sweepCardHtml();
      html = `<div class="note-bar">Tier 2 · ${cfgStr('batchLabel') ? 'cleared ' + esc(cfgStr('batchLabel')) : 'batched'}</div>` + sw + (list.length ? list.map((it, i) => cardHtml(it, i)).join('') : (sw ? '' : `<div class="empty">Batch is clear.</div>`));
    } else if (view === 'weekly') {
      const unsub = sweep && Array.isArray(sweep.unsubscribe) ? sweep.unsubscribe : [];
      const unsubHtml = unsub.length ? `<h2>Unsubscribe suggestions</h2><div class="card"><div class="body ulist">${unsub.map((u) => `<div class="krow"><span class="ss">${esc(u.sender || '')}</span><span class="sf">${esc(u.reason || '')}</span></div>`).join('')}</div></div>` : '';
      html = `<div class="note-bar">Tier 3 · review once a week, kill freely</div>` + (list.length ? list.map((it, i) => cardHtml(it, i)).join('') : `<div class="empty">Nothing here.</div>`) + unsubHtml;
    } else {
      html = list.length ? list.map((it, i) => cardHtml(it, i)).join('') : `<div class="empty">Nothing handled yet.</div>`;
    }
    $('#list').innerHTML = html;
  }

  function readDraft(card, it) {
    const base = draftFor(it);
    if (!base || !card) return base;
    const d = Object.assign({}, base);
    card.querySelectorAll('[data-f]').forEach((el) => { const f = el.dataset.f; if (f !== 'note') d[f] = el.value; });
    return d;
  }

  function nextBatch() {
    const d = new Date(); d.setHours(7, 0, 0, 0);
    const days = batchDays();
    let guard = 0;
    do { d.setDate(d.getDate() + 1); guard++; } while (!days.includes(d.getDay()) && guard < 8);
    return d.toISOString();
  }

  function logDecision(entry) {
    if (!db) return;
    const day = new Date().toISOString().slice(0, 10);
    const ref = db.doc('decisions/' + day);
    logChain = logChain.then(async () => {
      const snap = await ref.get();
      if (snap.exists) {
        const prev = Array.isArray(snap.data().entries) ? snap.data().entries : [];
        await ref.update({ entries: prev.concat([entry]) });
      } else {
        await ref.set({ date: day, entries: [entry] });
      }
    }).catch(() => {});
  }

  // ---- Connector calls ----
  // Returns {ok:true, link} | {ok:false, queue:true, msg} (call never ran: fall back to the sweep)
  //                         | {ok:false, queue:false, msg} (tool ran and failed, or outcome unknown: leave it open)
  function explain(e, server) {
    const code = e && e.code;
    if (code === 'needs_reauth') return { queue: true, msg: `Reconnect ${server} in claude.ai Settings → Connectors. Queued for the next sweep instead.` };
    if (code === 'server_not_connected') return { queue: true, msg: `${server} isn't connected for this page. Queued for the next sweep instead.` };
    if (code === 'selection_required') return { queue: true, msg: `Pick which ${server} connector to use when claude.ai asks. Queued for the next sweep instead.` };
    if (code === 'not_in_manifest' || code === 'approval_required' || code === 'consent_required') return { queue: true, msg: `${server} wasn't allowed for this page. Queued for the next sweep instead.` };
    if (code === 'blocked_by_policy') return { queue: true, msg: `Your org blocks this ${server} action here. Queued for the next sweep instead.` };
    if (code === 'not_granted' || code === 'capability_disabled' || code === 'capability_removed') return { queue: true, msg: 'Instant drafts are off in this view. Queued for the next sweep instead.' };
    if (code === 'tool_error') {
      const m = String((e && e.message) || '');
      if (/attached_draft_exists/.test(m)) return { queue: false, msg: `A draft already exists in that ${chatApp()} conversation. Edit or delete it there, then try again.` };
      return { queue: false, msg: `${server} refused the draft: ${m.slice(0, 140) || 'no reason given'}` };
    }
    return { queue: false, msg: `Couldn't confirm the ${server} draft was created. Check your drafts before trying again.` };
  }

  async function createDraft(d) {
    if (isChat(d)) {
      if (!slackServer()) return { ok: false, queue: true, msg: '' };
      const input = { channel_id: d.channelId, message: d.body || '' };
      if (d.threadTs) input.thread_ts = String(d.threadTs);
      try {
        const r = await mcp.callTool(slackServer(), 'slack_send_message_draft', input, { cache: false });
        const p = (r && r.payload) || {};
        return { ok: true, link: safeUrl(p.channel_link) };
      } catch (e) { return Object.assign({ ok: false }, explain(e, chatApp())); }
    }
    if (d.kind === 'email') {
      if (!gmailServer()) return { ok: false, queue: true, msg: '' };
      const input = { to: emailsIn(d), subject: d.subject || '', body: d.body || '', includeSignature: true };
      if (Array.isArray(d.cc) && d.cc.length) input.cc = d.cc;
      if (d.replyToMessageId) input.replyToMessageId = String(d.replyToMessageId);
      try {
        const r = await mcp.callTool(gmailServer(), 'create_draft', input, { cache: false });
        const p = (r && r.payload) || {};
        return { ok: true, link: safeUrl(p.viewUrl) };
      } catch (e) { return Object.assign({ ok: false }, explain(e, emailApp())); }
    }
    return { ok: false, queue: true, msg: '' };
  }

  function delegateBody(it, name) {
    const links = (Array.isArray(it.sources) ? it.sources : []).map((s) => safeUrl(s && s.url) ? `${s.label || 'Link'}: ${s.url}` : '').filter(Boolean);
    const parts = [`Hi ${name}, could you take this one?`, `*${it.title || ''}*${it.rec ? '\n' + it.rec : ''}`];
    if (it.why) parts.push(`Why now: ${it.why}`);
    if (links.length) parts.push(links.join('\n'));
    parts.push('Thanks!');
    return parts.join('\n\n');
  }

  async function act(id, kind, arg) {
    const it = items.find((i) => i.id === id);
    if (!it || !db || busy.has(id)) return;
    const card = document.querySelector(`.card[data-id="${CSS.escape(id)}"]`);
    const noteEl = card && card.querySelector('[data-f="note"]');
    const note = noteEl ? noteEl.value.trim() : '';
    const draft = readDraft(card, it);
    const edited = !!(draft && it.draft && JSON.stringify(draft) !== JSON.stringify(it.draft));
    const patch = { updatedAt: nowIso() };
    if (note) patch.note = note;
    if (draft && edited) { patch.draft = draft; patch.edited = true; }
    let msg = '';
    busy.add(id); render();
    try {
      if (kind === 'approve') {
        const target = draft && draft.kind !== 'doc' ? instantTarget(draft) : null;
        if (target) {
          toast(`Creating the ${target.server} draft…`, 8000);
          const r = await createDraft(draft);
          if (r.ok) {
            patch.status = 'done'; patch.draftCreatedAt = nowIso(); patch.draftedFrom = 'page';
            if (r.link) patch.draftLink = r.link;
            msg = `${target.server} draft is ready. Review and send it there.`;
          } else if (r.queue) {
            patch.status = 'approved'; msg = r.msg;
          } else {
            await db.doc('items/' + id).update(patch);
            toast(r.msg, 5000);
            return;
          }
        } else {
          patch.status = 'approved';
          msg = draft && draft.kind !== 'doc' ? 'Approved. The next sweep creates the draft.' : 'Marked done';
        }
      }
      if (kind === 'delegate') {
        patch.status = 'delegated'; patch.delegateTo = arg;
        const userId = people()[arg];
        if (mcp && userId && slackServer()) {
          toast(`Drafting the ${chatApp()} DM to ${arg}…`, 8000);
          const r = await createDraft({ kind: 'chat', channelId: userId, body: delegateBody(it, arg) });
          if (r.ok) {
            patch.status = 'done'; patch.draftCreatedAt = nowIso(); patch.draftedFrom = 'page';
            if (r.link) patch.draftLink = r.link;
            msg = `${chatApp()} DM draft to ${arg} is ready. Review and send it there.`;
          } else if (r.queue) {
            msg = r.msg;
          } else {
            patch.status = it.status || 'open'; delete patch.delegateTo;
            await db.doc('items/' + id).update(patch);
            toast(r.msg, 5000);
            return;
          }
        } else {
          msg = `Delegating to ${arg}. The next sweep drafts the ${chatApp()} DM.`;
        }
      }
      if (kind === 'kill') { patch.status = 'killed'; msg = 'Killed'; }
      if (kind === 'snooze') { patch.status = 'snoozed'; patch.snoozeUntil = arg === 'batch' ? nextBatch() : new Date(Date.now() + Number(arg) * 864e5).toISOString(); msg = 'Snoozed'; }
      if (kind === 'retier') { patch.tier = arg; patch.retieredFrom = it.tier; msg = `Moved to ${arg}`; }
      if (kind === 'reopen') { patch.status = 'open'; msg = 'Reopened'; }
      await db.doc('items/' + id).update(patch);
      delete drafts[id];
      pick = null;
      if (kind !== 'retier' && kind !== 'reopen') openId = null;
      logDecision({ itemId: id, title: it.title || '', action: kind, arg: arg || null, fromTier: it.tier, edited, note, drafted: !!patch.draftCreatedAt, at: patch.updatedAt });
      toast(msg, msg.length > 60 ? 4500 : 2400);
    } catch (e) {
      toast(e && e.code === 'invalid_argument' ? "Couldn't save: you may not have write access." : "Couldn't save. Try again.");
    } finally { busy.delete(id); render(); }
  }

  // ---- Tell Claude: save the owner's note on the item, then start the routine with it ----
  function notesText(where, text) {
    return `Tell Claude (notes only), from the queue page. ${ownerName()} left a note on ${where}:\n\n${text}\n\nRun the notes-only path of the routine: answer every pending note (items with askPending true and asks with status "new"), then stop. No sweep, no new Top 3 unless a note asks for it, no summary notification.`;
  }
  function notesRunActive() {
    return notesRun && notesRun.status === 'running' && notesRun.startedAt && (Date.now() - new Date(notesRun.startedAt)) < NOTES_STALE_MS;
  }
  // Starts one notes-only run. If one is already running, it picks up the new note before it finishes.
  async function fireNotes(where, text) {
    if (!mcp || !runAvailable || !triggerId()) return 'saved';
    if (notesRunActive()) return 'joined';
    try {
      await mcp.callTool(REMOTE, 'fire_trigger', { trigger_id: triggerId(), text: notesText(where, text) }, { cache: false });
    } catch (e) { return 'saved'; }
    try { await db.doc('meta/notesRun').set({ status: 'running', startedAt: nowIso(), startedBy: 'page' }); } catch (e) {}
    return 'fired';
  }
  function notesToast(r) {
    if (r === 'fired') return 'Sent. Claude replies here in a few minutes.';
    if (r === 'joined') return 'Sent. Claude is already working through notes and will pick this one up.';
    return 'Saved. The next sweep answers it here.';
  }
  async function askClaude(id) {
    const it = items.find((i) => i.id === id);
    if (!it || !db || busy.has(id)) return;
    const el = document.getElementById('ask-' + id);
    const text = ((el ? el.value : askDrafts[id]) || '').trim();
    if (!text) { toast('Type a note for Claude first.'); return; }
    const at = nowIso();
    const thread = (Array.isArray(it.thread) ? it.thread : []).concat([{ from: 'owner', text, at }]);
    busy.add(id); render();
    try {
      await db.doc('items/' + id).update({ thread, askPending: true, askedAt: at, updatedAt: at });
      delete askDrafts[id]; pick = null;
      logDecision({ itemId: id, title: it.title || '', action: 'ask', arg: null, fromTier: it.tier, note: text, at });
      const r = await fireNotes(`items/${it.id} ("${it.title || ''}")`, text);
      toast(notesToast(r), 4500);
    } catch (e) {
      toast("Couldn't save. Try again.");
    } finally { busy.delete(id); render(); }
  }

  // ---- Run sweep now ----
  function lastRunAt() { try { return Number(localStorage.getItem('queue.lastRunNow') || 0); } catch (e) { return 0; } }
  function setLastRun(t) { try { localStorage.setItem('queue.lastRunNow', String(t)); } catch (e) {} }
  function renderRun() {
    const b = $('#runNow');
    b.hidden = !runAvailable || !triggerId();
    if (b.hidden) return;
    const cooling = Date.now() - lastRunAt() < RUN_COOLDOWN_MS;
    b.disabled = runBusy || cooling;
    b.textContent = runBusy ? 'Starting…' : cooling ? 'Sweep running…' : 'Run sweep now';
  }
  async function runNow() {
    if (!mcp || runBusy || !triggerId()) return;
    runBusy = true; renderRun();
    try {
      await mcp.callTool(REMOTE, 'fire_trigger', { trigger_id: triggerId() }, { cache: false });
      setLastRun(Date.now());
      toast('Sweep started. Approvals become drafts and the queue refreshes in about 5 minutes.', 5000);
      setTimeout(renderRun, RUN_COOLDOWN_MS + 1000);
    } catch (e) {
      const code = e && e.code;
      if (code === 'needs_reauth') toast('Reconnect Claude Code Remote in claude.ai Settings → Connectors.', 5000);
      else if (code === 'not_in_manifest' || code === 'approval_required' || code === 'consent_required') toast('Running the sweep needs your OK for this page. Try again and allow it.', 5000);
      else if (code === 'server_not_connected' || code === 'blocked_by_policy' || code === 'not_granted' || code === 'capability_disabled') { runAvailable = false; toast("Run sweep isn't available here. Your scheduled sweeps still run.", 5000); }
      else if (code === 'tool_error') toast(`The sweep didn't start: ${String((e && e.message) || '').slice(0, 120)}`, 5000);
      else toast("Couldn't confirm the sweep started. Wait a few minutes before trying again.", 5000);
    } finally { runBusy = false; renderRun(); }
  }

  document.addEventListener('click', (e) => {
    if (e.target.closest('#runNow')) { runNow(); return; }
    const tab = e.target.closest('[data-view]');
    if (tab) { view = tab.dataset.view; openId = null; pick = null; sel = 0; render(); return; }
    const el = e.target.closest('[data-act]');
    if (!el || el.disabled) return;
    const card = el.closest('.card'); if (!card) return;
    const id = card.dataset.id;
    const a = el.dataset.act;
    if (a === 'toggle') { openId = openId === id ? null : id; pick = null; sel = Number(card.dataset.idx || 0); render(); return; }
    if (a === 'pick') { pick = (pick && pick.id === id && pick.kind === el.dataset.kind) ? null : { id, kind: el.dataset.kind }; render(); return; }
    if (a === 'sweepapply') { applySweep(); return; }
    if (a === 'sweepskip') { skipSweep(); return; }
    if (a === 'ask') { askClaude(id); return; }
    if (id.startsWith('w:')) { waitAct(id, a, el.dataset.arg); return; }
    act(id, a, el.dataset.arg);
  });
  document.addEventListener('change', (e) => {
    const t = e.target;
    if (!t || !t.dataset || !t.dataset.sweep) return;
    if (t.checked) sweepOff.delete(t.dataset.sweep); else sweepOff.add(t.dataset.sweep);
    const b = document.querySelector('[data-act="sweepapply"]');
    const n = sweepActionable().filter((x) => !sweepOff.has(x.threadId)).length;
    if (b) { b.textContent = `${mcp && gmailServer() ? 'Apply' : 'Approve'} ${n}`; b.disabled = !n || sweepBusy; }
  });

  document.addEventListener('input', (e) => {
    if (e.target.id && e.target.id.startsWith('ask-')) { askDrafts[e.target.id.slice(4)] = e.target.value; return; }
    const card = e.target.closest && e.target.closest('.card');
    if (!card || !e.target.dataset.f || e.target.dataset.f === 'note') return;
    const it = items.find((i) => i.id === card.dataset.id);
    if (it) drafts[it.id] = readDraft(card, it);
  });
  document.addEventListener('focusout', () => { setTimeout(() => { if (pendingRender) render(); }, 0); });

  document.addEventListener('keydown', (e) => {
    if (/INPUT|TEXTAREA/.test(e.target.tagName)) { if (e.key === 'Escape') e.target.blur(); return; }
    if (view === 'waiting') { if (e.key === 'Escape') { openId = null; pick = null; render(); } return; }
    const list = listFor(view);
    const cur = list[sel];
    if (e.key === 'j') { sel = Math.min(list.length - 1, sel + 1); render(); }
    else if (e.key === 'k') { sel = Math.max(0, sel - 1); render(); }
    else if (e.key === 'Enter' && cur) { openId = openId === cur.id ? null : cur.id; pick = null; render(); }
    else if (e.key === 'Escape') { openId = null; pick = null; render(); }
    else if (!cur) return;
    else if (e.key === 'a') { openId = cur.id; act(cur.id, 'approve'); }
    else if (e.key === 'x') { act(cur.id, 'kill'); }
    else if (e.key === 'd') { if (!hasPeople()) return; openId = cur.id; pick = { id: cur.id, kind: 'delegate' }; render(); }
    else if (e.key === 's') { openId = cur.id; pick = { id: cur.id, kind: 'snooze' }; render(); }
    else if (['1', '2', '3', '4'].includes(e.key)) { const t = TIERS[Number(e.key) - 1]; if (t !== cur.tier) act(cur.id, 'retier', t); }
  });

  function showMsg(t) { $('#list').innerHTML = `<div class="msg">${esc(t)}</div>`; }

  // ---- Inbox: quick adds the next sweep turns into queue items ----
  let inbox = [], adding = false;
  function renderInbox() {
    const pending = inbox.filter((r) => (r.status || 'new') === 'new').sort((a, b) => String(a.createdAt || '').localeCompare(String(b.createdAt || '')));
    $('#inbox').innerHTML = pending.length
      ? `<div class="hd">Waiting for the next sweep (${pending.length})</div>` + pending.map((r) => `<div class="row"><span>${esc(r.text)}</span><button type="button" data-inbox-del="${esc(r.id)}" aria-label="Remove">✕</button></div>`).join('')
      : '';
  }
  async function addToInbox() {
    const el = $('#addText');
    const text = el.value.trim();
    if (!text || !db || adding) return;
    adding = true; $('#addBtn').disabled = true;
    const id = 'in-' + Date.now().toString(36) + '-' + Math.random().toString(36).slice(2, 6);
    try {
      await db.doc('inbox/' + id).set({ text, status: 'new', createdAt: nowIso() });
      el.value = '';
      toast(runAvailable && triggerId() ? 'Added. Run sweep now to file it, or the next scheduled sweep will.' : 'Added. The next sweep files it.', 3500);
    } catch (e) {
      toast("Couldn't add that. Try again.");
    } finally { adding = false; $('#addBtn').disabled = false; }
  }
  $('#addForm').addEventListener('submit', (e) => { e.preventDefault(); addToInbox(); });
  $('#addText').addEventListener('keydown', (e) => { if (e.key === 'Enter' && !e.shiftKey) { e.preventDefault(); addToInbox(); } });
  $('#inbox').addEventListener('click', async (e) => {
    const b = e.target.closest('[data-inbox-del]');
    if (!b || !db) return;
    b.disabled = true;
    try { await db.doc('inbox/' + b.dataset.inboxDel).delete(); } catch (err) { b.disabled = false; toast("Couldn't remove it. Try again."); }
  });

  // ---- Tell Claude about the queue as a whole (Top 3, rules, anything not tied to one item) ----
  function renderAsks() {
    const cutoff = Date.now() - 48 * 3600e3;
    const show = asks.filter((a) => !a.dismissed && ((a.status || 'new') === 'new' || (a.answeredAt && new Date(a.answeredAt) > cutoff)))
      .sort((a, b) => String(b.createdAt || '').localeCompare(String(a.createdAt || ''))).slice(0, 4);
    $('#asks').innerHTML = show.map((a) => `<div class="a"><button type="button" class="x" data-ask-dismiss="${esc(a.id)}" aria-label="Dismiss">✕</button>
      <div class="msgk"><span class="msgw">You${a.createdAt ? ' · ' + esc(shortDate(a.createdAt)) : ''}</span>${esc(a.text || '')}</div>
      ${(a.status || 'new') === 'new' ? `<div class="pending">Claude is working on this…</div>` : `<div class="msgc"><span class="msgw">Claude</span>${esc(a.reply || 'Done.')}</div>`}</div>`).join('');
  }
  async function sendAsk() {
    const el = $('#askTextTop');
    const text = el.value.trim();
    if (!text || !db || asking) return;
    asking = true; $('#askBtnTop').disabled = true;
    const id = 'ask-' + Date.now().toString(36) + '-' + Math.random().toString(36).slice(2, 6);
    try {
      await db.doc('asks/' + id).set({ text, status: 'new', createdAt: nowIso() });
      el.value = '';
      logDecision({ askId: id, action: 'ask', arg: null, note: text, at: nowIso() });
      const r = await fireNotes(`the queue as a whole (asks/${id})`, text);
      toast(notesToast(r), 4500);
    } catch (e) {
      toast("Couldn't send that. Try again.");
    } finally { asking = false; $('#askBtnTop').disabled = false; }
  }
  $('#askForm').addEventListener('submit', (e) => { e.preventDefault(); sendAsk(); });
  $('#askTextTop').addEventListener('keydown', (e) => { if (e.key === 'Enter' && !e.shiftKey) { e.preventDefault(); sendAsk(); } });
  $('#asks').addEventListener('click', async (e) => {
    const b = e.target.closest('[data-ask-dismiss]');
    if (!b || !db) return;
    b.disabled = true;
    try { await db.doc('asks/' + b.dataset.askDismiss).update({ dismissed: true }); } catch (err) { b.disabled = false; }
  });

  async function initMcp() {
    try { mcp = await window.claude.use('mcp'); } catch (e) { mcp = null; }
    if (!mcp) { render(); return; }
    try {
      const res = await mcp.listTools();
      const servers = (res && res.servers) || [];
      const remote = servers.find((s) => s.server === REMOTE);
      runAvailable = !!(remote && (remote.tools || []).some((t) => t.name === 'fire_trigger'));
    } catch (e) { runAvailable = false; }
    render();
  }

  async function init() {
    if (!window.claude || typeof window.claude.use !== 'function') { showMsg('Open this page on claude.ai to load your queue.'); return; }
    initMcp();
    db = await window.claude.use('db');
    if (!db) { showMsg('Sign in to claude.ai to load your queue.'); return; }
    const timer = setTimeout(() => { if (!loaded) showMsg("Can't reach the queue yet. It'll load when the connection comes back."); }, 10000);
    db.collection('items').onSnapshot((snap) => {
      items = snap.docs.map((d) => Object.assign({ id: d.id }, d.data()));
      if (!snap.metadata.fromCache) { loaded = true; clearTimeout(timer); }
      render();
    }, () => { clearTimeout(timer); loaded = true; showMsg("The queue stopped syncing. Reload the page to reconnect."); });
    db.doc('meta/today').onSnapshot((snap) => { meta = snap.exists ? snap.data() : {}; render(); }, () => {});
    db.doc('meta/config').onSnapshot((snap) => {
      config = snap.exists ? snap.data() : {};
      const nm = cfgStr('name');
      if (nm) { document.title = nm + "'s Queue"; $('h1').textContent = nm + "'s Queue"; }
      render();
    }, () => {});
    db.collection('inbox').onSnapshot((snap) => { inbox = snap.docs.map((d) => Object.assign({ id: d.id }, d.data())); renderInbox(); }, () => {});
    db.collection('waits').onSnapshot((snap) => { waits = snap.docs.map((d) => Object.assign({ id: d.id }, d.data())); render(); }, () => {});
    db.doc('sweeps/current').onSnapshot((snap) => { sweep = snap.exists ? snap.data() : null; render(); }, () => {});
    db.doc('meta/notesRun').onSnapshot((snap) => { notesRun = snap.exists ? snap.data() : {}; }, () => {});
    db.collection('asks').onSnapshot((snap) => { asks = snap.docs.map((d) => Object.assign({ id: d.id }, d.data())); renderAsks(); }, () => {});
  }
  renderTabs();
  init();
})();
</script>

</body></html>
</body>
</html>
```

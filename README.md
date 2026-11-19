# Chief of staff for Claude

A personal chief of staff that runs inside Claude. It sweeps your email, chat, meeting notes and work tracker a few times a day, ranks everything against your goals for the quarter, and gives you one short queue with a recommendation and a ready draft on each item.

You approve, delegate, snooze, re-tier or kill. Nothing ever sends on its own: every message is a draft you review and send yourself.

![Demo](chief-of-staff-demo.gif)

## How it learns

Every override you make gets logged with why: a re-tier, a kill, a "this isn't mine." You can also tell Claude what to change on any item, and it checks your tools, redrafts and answers right on the card. Every two weeks it interviews you about those overrides and rewrites its own priority rubric, so it needs fewer corrections over time.

## What you get

- **A priorities skill** with your goals, four tiers, routing rules and standing rules, in your words
- **A private queue page** on phone and laptop: Top 3, a batch for routine work, a weekly noise list, and a Waiting tab for what others owe you
- **A scheduled chief of staff** that sweeps your tools, ranks new items, answers your notes and drafts follow-ups
- **A biweekly check** that tunes the rubric from how you actually used it

## What you need

- A paid Claude plan with scheduled tasks and artifacts
- At least one tool connected in Claude. It works with whatever you use:
  - Email: Gmail or Outlook / Microsoft 365
  - Chat: Slack or Microsoft Teams
  - Calendar: Google Calendar or Outlook
  - Meeting notes: Granola, Otter, Fireflies, Fathom, Zoom or Teams
  - Tracker or CRM: Linear, Jira, Asana, GitHub, Notion, HubSpot or Salesforce

With Gmail and Slack, approving an item creates the draft instantly from the page. With other tools, the next scheduled run creates it.

## Setup (about 20 minutes)

1. Download [`chief-of-staff-setup.md`](chief-of-staff-setup.md).
2. Open a new Claude chat, attach the file, and send:

   > Create a skill from the SKILL.md section of this file, then run it to set me up.

3. Answer the questions. Most are tappable.
4. Save the priorities skill when Claude proposes it.

Everything it creates is private to you. To pause it, turn off the two scheduled tasks.

## License

MIT

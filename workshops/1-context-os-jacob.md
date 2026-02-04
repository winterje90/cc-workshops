# Workshop 1 (Jacob): Context OS Starter Kit for GTM Teams

## Who it's for
GTM leaders, RevOps, product marketing, founders, and operators who want a repeatable way to store and reuse customer context.

## Learning goals
By the end, participants can:
- Set up a local workspace (VS Code or Cursor) with Claude Code and work in a repo they can version.
- Turn exported sales call transcripts into a small set of "context packages" (structured files you can work from repeatedly).
- Use the context package for onboarding ("talk to the company brain" to get up to speed on ICPs and more).
- Know where this approach works well and where it breaks down (maintenance overhead as it grows).

## Pre-work (send 48–72 hours before)
Participants should:
- Install VS Code or Cursor + Claude Code extension.
- Install Node + Git (baseline setup).
- Export ~15 recent sales call transcripts (separate files is fine; do it ahead of time because export is annoying).
- Clone/pull the quick start repo (for setup + saving work).
- Ask Claude to take your sales deck and messaging docs and convert to markdown

## 90-minute agenda

**0–10 min — Mental model: it's files + a UI**
- VS Code/Cursor is mainly a way to see files. The "agent" can act on those files.
- CAUTION: Auto-save doesn't apply here, this is both good and bad. This is why we use Github. 
- Why terminal-only is risky: you can't see what it's doing.
- Why this is not just a custom GPT or a Claude Project. 

**10–25 min — What the Context OS is**
- Centralize working docs and transcripts into a knowledge base you can always work from, reducing context switching and repeated explainers.
- Frame it as "company brain" and "context packages."
- How you'll use this: PMM, content creation. Think about PMM, it's a role where you collect context across your market, the competition, product, etc... and use all of that to inform what copy/messaging should be. 

**25–40 min — Live build: create your first context package**
- Build your first global Claude.md 
- Create a folder and import transcripts. (Small batch: enough to "make it click.")
- Jacob to ask everyone to type this into CC/Terminal: "what meaningful information can be extracted from these calls"
- Generate 2–4 starter artifacts:
  - Personas 
  - top themes / objections
  - "what we do" narrative you can reuse in docs and onboarding

**40–55 min — Hands-on sprint: everyone produces one artifact**
Pick one output:
- 1-page ICP brief (from transcripts)
- "top objections + best responses"
- onboarding starter doc ("ask the company brain" prompts)

Facilitator roams and helps people get unstuck.

**55–70 min — Onboarding flow**
- Show the onboarding use case: new hires talk to the system to get up to speed on ICPs and context.

**70–82 min — Limitations and when it gets hard**
- It's effective, but maintenance gets harder as you add more and more.
- Batch processing at big scale takes time you don't have, so keep live exercises small.

**82–90 min — Governance: make it survive contact with reality and what Jacob does**
- You need an arbiter / "benevolent dictator" to keep it from falling apart.
- Jacob builds these systems for companies and helps them maintain them over time

## Hands-on activities
- Import transcript exports into a repo and generate 1–2 reusable docs.
- Draft an onboarding prompt set (what a new hire should ask first).

## Key takeaways
- Seeing and organizing the file system is half the battle.
- Start small with a handful of transcripts. Build from there.
- Someone must own quality and structure.

## Optional Enhancements
- Add a weekly 15-minute "context refresh" ritual.
- Add tags (segment, industry, objections) and a simple naming convention.
- Add a review loop: one person generates, another edits for accuracy and clarity.

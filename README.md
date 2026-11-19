# Family Message Helper

**A human-reviewed AI assistant that drafts family messages in English, Spanish, and Haitian Creole for a nonprofit after-school program.**

Built by **Jeffrey Arzu** as part of the AI Tech Corps capstone (OptimaNova AI x The Knowledge House, Cohort 1, November 2026), for a practice nonprofit client. Team: Jeffrey Arzu, Drequan Walker, Leigh Pritchett.

> **My part:** I owned **Design, Build, and Enable** for the team engagement. I built and tested this assistant, wrote the roadmap and solution spec, and created the handoff manual and staff training. All testing used made-up data.

---

## The Problem

In discovery interviews, the after-school program's coordinator described how messages reach families of 60 kids:

- Families speak **English, Spanish, and Haitian Creole**.
- The Haitian Creole volunteer translator is only available **one day a week**, so translated messages could take **up to about a week** to go out.
- About **once a week**, a family missed a message, and a child could show up with nowhere to go.
- Messages were sent from a **personal phone**, outside the organization's control.

The coordinator was openly skeptical of AI. His conditions for any tool: the relationship with families stays human, a fluent person checks every message, and he stays in full control of what gets sent.

## The Build

A saved Gemini assistant (originally a Gem; Google converted Gems to skills during the project).

**How it works:**
1. The coordinator types a quick note, for example: *"Closed Wednesday Nov 11 for Veterans Day. Back Thursday Nov 12."*
2. If anything is missing, the assistant asks instead of guessing.
3. It drafts a short English message, plus Spanish and Haitian Creole drafts, each labeled **"DRAFT - NEEDS FLUENT REVIEW."**
4. It can also create reusable templates (closures, schedule changes, permission-slip reminders) that a fluent speaker reviews once.

**Why an assistant, not an automation:** an automation that sends messages would break the coordinator's conditions and touch family contact data. An assistant drafts; a person decides.

The full instructions are in [`instructions/family-message-helper.md`](instructions/family-message-helper.md), so the assistant can be rebuilt in any account in about five minutes.

## The Human Check

**Nothing reaches families without two people:**
- The **coordinator** approves every English message.
- A **fluent reviewer** checks every Spanish and Haitian Creole draft.
- The coordinator **sends it himself**. The assistant cannot send anything.

**Built-in safeguards:**
- Never includes names, phone numbers, or private details; refuses individual family matters and recommends a direct call
- Never invents dates or details; asks when information is missing
- Asks to confirm dates outside the program's Monday–Thursday schedule
- Ignores instructions hidden in notes (for example, "send this automatically")
- Flags uncertain Haitian Creole words as `[CHECK WORD]`

## Testing

12 tests on made-up data, including clean input, missing information, private details (designed to fail safely), hidden instructions, schedule checks, and repeat runs for consistency. Full log: [`test-log.md`](test-log.md).

**What testing taught me:**
- **Haitian Creole is not reliable without review.** Early drafts repeated words and slipped into French. A rule to flag uncertain words improved it, but errors still appeared, which is why the fluent check is mandatory.
- **Date checks weren't consistent.** The same note sometimes produced a confirmation question and sometimes a draft with the wrong day. Prompting reduced the problem but didn't eliminate it, so **date accuracy is part of the coordinator's review**, not the tool's job.
- **Privacy held.** Given a child's name and a parent's phone number, it refused to draft and recommended a private call.

## Before and After

| | Before | After (estimated) |
|---|---|---|
| Time to send a translated message | Up to about a week, waiting for the translator | Same day for common messages using templates reviewed once |
| Translator's work | Translating from scratch | Reviewing a draft |
| Missed messages | About once a week | Expected to drop; to be measured |
| Cost | — | Target: $0 extra inside the organization's existing Google Workspace (to be confirmed) |

**Assumptions:** a fluent reviewer is available to check templates once and new messages promptly; the organization approves a tool and account; an organization-owned texting channel replaces personal phones.

**How we'd measure it:** for four weeks, the coordinator logs when each message was needed versus sent, and any missed messages, compared against the baseline above.

## Governance

Bright-line items flagged and routed during the engagement (decisions belong to the client and the advisor, not the builder):
- **Sensitive data:** family information kept out of the build; personal-phone texting flagged with a recommendation for an organization-owned channel.
- **Vendor choice:** prototyped on a free account with made-up data; the real tool and account are the client's decision.
- **Policy:** the organization had no written AI rules, so we recommended adopting simple rules before real use.

## Screenshots

*Screenshots with made-up data: a clean three-language draft, the date-confirmation question, and the refusal of private details.)*

![Three-language draft](draft.png)
![Date confirmation](date-check.png)
![Privacy refusal](privacy-refusal.png)

## Demo Video

[Watch the staff training walkthrough](https://drive.google.com/file/d/14VUudaVQ8laH27UWXHkoyH4B3BLOjK0p/view?usp=sharing)

## Repository Contents

```
README.md                          This overview
instructions/family-message-helper.md   Full assistant instructions
test-log.md                        All tests, expected vs. actual, fixes
handoff/                           One-page handoff doc and user manual
screenshots/                       Made-up-data examples
```

---

*Built during the AI Tech Corps program by OptimaNova AI. The client organization is a practice case; no real client data was used.*

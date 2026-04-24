---
name: project-next-advisor
description: "Produces a \"what to work on next\" advisory for the current project by synthesising the codebase state, recent conversation history, and the most recent meeting notes. Always generates two tiers: concrete atomic tasks (detailed enough to just go do) and larger vision items (directional, ambitious). Trigger when the user says things like \"what should I work on\", \"what's next\", \"what should I focus on\", \"give me next steps\", \"any ideas\", or similar requests for prioritisation or direction. Also trigger when the user seems stuck or is asking for creative input on the project."
---

# Project Next Advisor

Answers: "What should I work on next, and why?"

Two tiers, always:
- **Concrete tasks** — specific, atomic, implementable, with enough detail to just go do them
- **Grand vision items** — larger directional ideas, genuinely ambitious, more hand-wavy

---

## Step 1: Gather Context

Do all of this before writing anything.

### 1a. Find the Most Recent Meeting Notes

This is the step most likely to go wrong. Meeting notes go stale fast — notes from two months ago
are nearly useless compared to last week's. Before using any meeting content, check the date.

```
recent_chats: n=20, sort_order=desc
```

Scan the results for chats titled "Meeting notes from VTT transcription" or similar. Take the
most recent 1–3 by date. Discard anything older than 6 weeks unless nothing more recent exists.
If uncertain whether a chat is a meeting note, search directly:

```
conversation_search: "meeting notes John Bustard weekly"
```

From those notes, extract:
- What did John flag as high-leverage or strategically important?
- What tensions or pivots were discussed?
- What action items or open questions came out of the meeting?
- Has anything from those items already been addressed? Cross-reference with recent chat history
  before treating them as open.

Meeting notes are loose directional signals, not mandates. Translate them into implications —
do not repeat them back.

### 1b. Recent Conversation History

```
recent_chats: n=10, sort_order=desc
```

Look for:
- What has the user been actively building or discussing in the last 1–2 weeks?
- Any features, designs, or concepts that were sketched but not fully implemented?
- Problems or frustrations that came up repeatedly?
- Threads that were dropped mid-conversation?

Recent chat history reflects actual momentum. Weight it heavily.

### 1c. Project Knowledge — Current State

```
project_knowledge_search: "features implemented current state"
project_knowledge_search: "technical debt known issues bugs"
project_knowledge_search: [any specific topic that came up in 1a or 1b]
```

This is ground truth for what is actually built. Use it to aggressively filter suggestions:
if it is already shipped, do not suggest it. If it is partially built, it is a strong candidate
for a concrete task. If it is only mentioned in meeting notes but not in the codebase or recent
chats, treat it as genuinely open.

---

## Step 2: Synthesise Before Writing

Build a clear picture of:

- **Current momentum**: What is the user actually in the middle of? Suggestions that work with the
  grain of recent activity are more useful than pivots.
- **Genuine gaps**: What was planned, discussed, or partially built but hasn't landed yet?
- **Strategic implications**: What did the most recent meeting point toward, and what does that
  mean concretely — not as a restatement, but as a derived next action?
- **Fragile areas**: Anything in the codebase known to be brittle or causing repeated problems?
- **Quick leverage**: Is there something small that would meaningfully change the user's experience
  of the product or workflow?

Only proceed to Step 3 once you have a concrete answer to each of these.

---

## Step 3: Write the Output

### Tone

Write as a knowledgeable colleague who has been watching the project closely, sat in on the
meetings, and read the code. The user knows their own product intimately — do not tell them things
they already know. Assume full context and skip the setup. The writing should feel like a
conversation between two people who are both invested in the outcome, not a report being handed
down.

Analytical and direct. Warm but not soft. Curious about the implications of things, not just
descriptive of them.

**Absolutely forbidden:**
- Performative short sentences used for dramatic effect — strings of clipped sentences that create
  false urgency or fake momentum ("The data is stored. The calculation runs.") read as hollow
- "Done looks like:" as a structural label
- "Why now:" as a structural label
- Restating things the user obviously already knows about their own project
- Summarising back to the user something they just built, as if they've forgotten
- Filler phrases: "it's worth noting", "importantly", "at the end of the day", "this is key"
- "It's not X, it's Y" constructions and all derivatives
- Reduce use of em-dashes — commas and colons almost always work better

British English throughout.

---

### Structure

#### Opening (1–3 sentences, optional)

Only include if there is something genuinely useful to say about the current moment — a tension,
a transition, a pivot point. Skip it entirely if it would just be scene-setting for its own sake.
Do not open by summarising what the user has been working on.

---

#### Concrete Tasks (3–5 items)

Each task gets a **headline** and **prose explanation**. No bullet sub-lists within a task.
No structural labels like "Done looks like:" or "Why now:". Just analytical prose covering: what
the task is, why it matters at this moment, and specifically how to approach it — referencing
actual files, systems, or patterns from the codebase where possible.

The headline should convey the point of the task, not just name it. "Fix persona name lookup" is
weak. "Close the gap between how personas are named and how they're referenced" is better.

The explanation should be specific enough that the user knows exactly where to start. Reference
real things: file names, system components, existing patterns. Generic descriptions are not useful.

Order: momentum alignment first (what continues current work naturally), then highest leverage,
then ease.

---

#### Grand Vision Items (2–3 items)

Each gets a **title** that sounds genuinely exciting — not a backlog item, not a project
management label. Something you'd actually want to read about.

The explanation (3–5 sentences) should describe what this could become and why it is worth
thinking about, connecting the current project to its larger ambition. Reference strategic framing
from recent meetings where it adds something, but push further. Even a vision item should contain
a real idea, not just an evocative wrapper around vagueness.

---

#### Closing (1–2 sentences, optional)

Only include if there is a genuine strategic tension, an upcoming decision point, or something
that didn't fit elsewhere and is worth flagging. Omit if there is nothing substantive to add.

---

## Hard Rules

- **Do not suggest things that are already built and shipped.** Project knowledge is the ground
  truth. Verify before suggesting.
- **Do not treat stale meeting notes as current priorities.** Always check the date. An action
  item from three months ago that hasn't surfaced since is probably done or deprioritised. Do not
  surface it as urgent.
- **Do not translate John's suggestions verbatim into tasks.** They are suggestive inputs. Derive the
  implication, add specificity, make it actionable.
- **Do not summarise the user's own recent work back to them.** They were there.
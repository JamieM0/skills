---
name: style-guide-creator
description: >
  Create a reusable writing style guide from scratch by eliciting the user's preferences through structured multiple-choice questioning. Use this skill whenever a user wants to define a new writing style, voice, or tone for a specific publication, persona, or context — for example, a blog, a newsletter, a product's AI-generated content, or a brand voice. Also use when someone wants to formalise an existing informal style, or when they say something like "I don't know what I want but I'll know it when I see it." This skill handles the full process: elicitation → draft → violation detection → iteration → final guide.
---

# Style Guide Creator

This skill replicates a proven preference-elicitation method for producing precise, reusable writing style guides. The process works by asking structured multiple-choice questions in rounds, generating sample output, catching violations (rules the user couldn't articulate until they saw them broken), and iterating into a finalised document.

The output is a Markdown style guide the user can store and reuse across conversations or projects.

---

## Step 0: Capture Context

Before writing a single question, establish the frame. Ask the user:

1. **What is this writing style for?** (e.g. a newsletter, AI-generated analysis, social posts, product documentation, a fictional newspaper's op-ed section)
2. **Who is the intended reader?** (technical, general public, industry insiders, a specific demographic)
3. **What is the writer's relationship to the reader?** (peer, expert-to-novice, journalist-to-citizen, brand-to-customer)
4. **Does a rough example exist?** Ask for a sample they like (any source), a sample they dislike, or a draft of something they want to sound like but don't yet. Any of these is useful; none is required.
5. **Are there hard constraints already known?** (e.g. British English, no em-dashes, specific vocabulary they already know they want to avoid)

If they can provide an existing bad draft and point out why it's bad, that's the most valuable input. The draft doesn't need to be good — it reveals instincts even when the result is wrong.

Confirm your understanding before moving to Round 1. Spend one short paragraph summarising what you'll be optimising for, then ask them to confirm or correct.

---

## Step 1: Round Structure

Tell the user explicitly: "I'll run [N] rounds of questions. Each round, answer with the letter(s) of your preferred option(s) and optionally a confidence level (e.g. 'A, 70%'). 50% is neutral, 100% is certain. After each round I'll generate a draft and refine the guide.". Let the user know that they don't need to give confidence levels - but it helps.

**Every answer option in every question across all rounds must include a concrete writing example.** This is non-negotiable. Abstract descriptions of style ("more formal", "shorter sentences") are insufficient — the user can't reliably commit to an abstraction they haven't seen applied. All options within a single question must use the same base scenario so the contrast is immediately visible without needing to mentally reconstruct what each option would produce.

Typical round count:
- **Minimum 3 rounds** for a basic guide
- **4–5 rounds** for a production-quality guide covering all edge cases
- More if the style is highly specialised or the user is finding lots of violations in drafts

Give the user control: let them say "ask more" or "that's enough for this round."

---

## Round 1: Tone and Voice

Cover the highest-level preferences first. Aim for 10–15 questions. Topics:

- Overall register (formal, conversational, authoritative, neutral)
- Relationship between writer and reader
- How confident/assertive the writing should be
- Whether to use first person, third person, or impersonal voice
- How to handle uncertainty (hedge explicitly, or omit hedged claims entirely)
- Acceptable use of humour, irony, or editorialising
- Whether the voice has personality or is deliberately transparent/neutral
- Language variety (single language, British/American English, mixed)
- Contractions (fine or not)

**Format each question as a lettered choice**, not open-ended. Open-ended questions during elicitation produce vague answers. Multiple-choice forces commitment.

**Each option must include a concrete writing example.** Pick a single base scenario and apply each option to it — the user should be able to compare directly without having to imagine what each approach would look like in practice. Keep examples short (one or two sentences); the contrast is what matters, not the content.

Example:
```
3. When expressing uncertainty about a claim, prefer:
   A: Omit uncertain claims — state only what's confirmed.
      *e.g. "Adoption has grown 40% year-on-year."*
   B: State the claim with an explicit upfront hedge.
      *e.g. "It's unclear whether this holds broadly, but adoption appears to have grown significantly."*
   C: State the claim, then qualify it in a follow-up sentence.
      *e.g. "Adoption has grown significantly. The figures vary by region and the methodology differs across sources."*
```

---

## Round 2: Sentence and Paragraph Structure

Cover:

- Sentence length (short and punchy, varied, longer and complex)
- How ideas connect between sentences (direct progression, transition words, connectors)
- Paragraph length
- How paragraphs open (conclusion first, context first, question first)
- Whether structure should vary by topic or stay consistent
- Heading style if applicable (descriptive, benefit-focused, bare noun)
- How technical implementation details are handled (explain, assume, omit)

---

## Round 3: Word Choice and Phrases

This round targets the specifics that differentiate styles at a surface level. Cover:

- Preferred intensity modifiers (e.g. "significantly" vs "really" vs none)
- How to introduce value (capability language, problem-solution, neutral description)
- How to refer to the system/product/author (first person, system name, impersonal)
- Acceptable and unacceptable words for common concepts
- Phrases to categorically avoid (ask explicitly — users often have strong feelings here)
- Clichés and jargon that feel wrong for the context

**Specifically prompt for anti-patterns.** Ask: "Are there any phrases that immediately ruin the feel of a piece for you?" Users often can't generate this list unprompted but recognise violations instantly.

---

## Round 4: Examples Round

Generate 4–6 pairs of "bad/good" example sentences or short paragraphs based on what you've learned. Ask the user to confirm whether each "good" example actually feels right, or whether something is still off.

This is the most important round for revealing unstated rules. If a "good" example feels wrong to the user, push them to articulate why — that explanation becomes a new rule.

Cover these types of contrasts:
- A sentence that opens a section well vs. one that doesn't
- A sentence describing value that works vs. one that sounds like marketing
- A technical explanation that's appropriately terse vs. over-explained
- A transition between paragraphs that works vs. one that feels artificial
- An opening sentence for an entire piece that sets the right tone vs. wrong tone

---

## Step 2: Generate Initial Draft

After Round 2 (minimum) or Round 3 (preferred), generate a sample piece (150–300 words) in the target context. This could be:

- A feature announcement if it's a product blog
- An analysis opening if it's a news/prediction format
- An opinion paragraph if it's an editorial voice
- Whatever the user's context calls for

Ask the user: "Does this feel right? Point at anything that doesn't." Don't ask for abstract feedback. Ask them to quote specific sentences or phrases that feel off and say what's wrong.

Each piece of negative feedback generates a new rule. Add it to the working guide immediately.

---

## Step 3: Violation Detection Loop

The most reliable source of rules is violations. After each draft:

1. Show the draft
2. Ask the user to mark anything that doesn't fit
3. Ask them to describe *why* it doesn't fit
4. Extract the rule from their explanation
5. Check for derivatives — if they hate "it's not X, it's Y", they probably also hate "the system doesn't X, it Y" and "not X but rather Y"

Rules discovered through violations should be:
- Named concisely
- Illustrated with a bad example and a good example
- Checked for derivative forms

---

## Step 4: Assemble the Style Guide

Once the user is satisfied with output quality, assemble the full guide in this structure:

```markdown
# [Name] Writing Style Guide

## Voice & Audience
- Writer persona
- Reader relationship  
- Tone description

## Core Principles
[The 3–6 most important rules, each with a bad/good example]

## Sentence Structure
[Length, progression, connection between ideas]

## Paragraph Structure
[Length norms, opening patterns, closing patterns]

## Word Choice
### Acceptable
### Avoid
### Phrases to Never Use
### Acceptable Explanatory Phrases

## Opening Patterns
[How to start pieces, sections, paragraphs]

## Structural Rules
[Headings, transitions, section format]

## Formatting
[Any specific formatting conventions]

## [Context-specific sections as needed]
[e.g. "Technical Descriptions", "Prediction Language", "Attribution Style"]

## Final Reminders
[A short list of the most common failure modes for this style]
```

Keep the guide descriptive, not prescriptive in tone. Annotate rules with brief reasoning where the reasoning is non-obvious. Use concrete bad/good examples throughout — they're more useful than abstract descriptions.

---

## Step 5: Validate Against Real Content

If the user has real content to write, write it now and use the guide. This is the highest-value test. Violations reveal:

- Rules that weren't explicit enough
- Rules that conflict with each other
- Edge cases the Q&A didn't surface

Update the guide with any new rules found.

---

## Reuse Pattern

The output guide is designed to be stored in a project and loaded at the start of writing sessions. When reusing the guide:

- Tell Claude to read the guide before writing
- Include the guide's name in the system prompt or project knowledge
- For AI-generated content pipelines, include the guide as a system prompt section or pass it in context

---

## Notes for Specific Use Cases

### Blog / Changelog Writing
Focus on the relationship between builder and reader. Cover how to describe improvements without hype, how to handle limitations honestly, and how to close posts (forward-looking vs. summary).

### Brand Voice
Focus Round 1 on personality and consistency across contexts. Add a section on how the voice adapts across formats (short social, long form, error states, UI copy).

### Multi-Author Guides
Add explicit rules about what can vary by author and what must stay consistent.

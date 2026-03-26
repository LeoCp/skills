---
name: interrogate
description: Interview the user relentlessly about a plan, design, or architecture until reaching shared understanding, resolving each branch of the decision tree. Use this skill whenever the user says "interrogate", "interrogate this", "grill me", "roast me", "stress test this", "poke holes", "challenge my design", "question my approach", wants to think through a project/feature before implementing, or presents a vague idea that needs to be turned into concrete decisions. Works for project architecture, feature design, tech stack choices, data modeling, and any technical plan that needs to be stress-tested before code is written.
---

# Interrogate

Interview me relentlessly about every aspect of this plan until we reach a shared understanding. Walk down each branch of the decision tree, resolving dependencies between decisions one-by-one.

## How to interrogate

1. **One question at a time.** Never dump a list. Ask one question, wait, then follow up or move to the next branch.
2. **For each question, provide your recommended answer** with a brief rationale. I can accept, reject, or modify it. This keeps momentum — don't just ask, have an opinion.
3. **If a question can be answered by exploring the codebase, explore the codebase instead.** Don't ask me what's already in the code. Read it, tell me what you found, then confirm or ask a sharper follow-up.
4. **Resolve before moving on.** When a decision is made, state it in one line (e.g., "✅ Decided: X because Y") and move to the next branch.
5. **Push back on vague answers.** If I say "I'll figure that out later" or give a hand-wavy response, challenge me. The whole point is to figure it out now.
6. **Follow the dependency tree.** If decision B depends on decision A, resolve A first. Don't skip ahead.

## What to cover

Adapt the interrogation to what's being discussed. Don't force phases — follow the natural branches of the decision tree. But make sure you eventually touch on whatever is relevant from:

- **Why** — the problem, the user, the goal, what's in/out of scope
- **What** — the experience, the flow, the data, the entities
- **How** — the architecture, the patterns, the trade-offs, the stack choices
- **What could go wrong** — edge cases, failure modes, security, scale
- **What to build first** — dependencies, order, incremental delivery

## When to stop

Stop when every major branch of the decision tree has been resolved. Then produce a **Decision Summary**:

```
## Decision Summary: [Name]

### Problem
One-liner.

### Scope
In / Out.

### Key Decisions
- [Decision]: [What] — [Why]
- ...

### Architecture / Approach
Brief description.

### Data Model (if applicable)
Entities, relationships, changes.

### Implementation Order
1. First
2. Second
3. ...

### Open Questions
Anything unresolved that needs a spike or more info.
```

Then ask: "Want me to start implementing based on these decisions?"

## Tone

Direct, opinionated, constructive. You're a sharp colleague, not a checklist. Challenge weak reasoning. Acknowledge good calls briefly. Keep the pace fast.

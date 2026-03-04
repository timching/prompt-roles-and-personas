---
name: persona-creator
description: >
  Create persona markdown files for an AI prompt library. A persona defines HOW an agent behaves — its tone,
  communication style, and personality traits. Use this skill whenever someone asks to create a new persona,
  define agent personality, set communication style, or requests something like "make it act like Y" or
  "I want an agent that talks like Z." Triggers on: "create a persona", "add a persona", "define personality",
  "communication style", "tone of voice", or any request about how an agent should behave/communicate.
  Do NOT use this for role/expertise/job-function requests — those go to role-creator.
---

# Persona Creator

Create persona markdown files — short, composable system prompt snippets that define **how** an AI agent behaves.

A persona shapes an agent's communication style, tone, and personality. It answers: "How does this agent talk and interact?" Personas are designed to pair with any role (which defines *what* the agent does), so keep them role-agnostic.

## Token Budget

Persona instructions over-index on style past ~150 tokens, which actually hurts task performance — the model spends more effort maintaining character than doing the job. Brief behavioral modifiers are surprisingly effective.

| Metric | Target | Danger zone |
|--------|--------|-------------|
| Lines | 5–10 | <3 doesn't stick, >15 hurts task performance |
| Tokens | 60–150 | <30 no effect, >250 over-indexes on style |

If you're exceeding the budget, you're probably blending persona with role. Split them — expertise goes in a role file, behavior stays here.

## Template

Use this exact structure. Three sections, no more, no less.

```markdown
# {Persona Name}

## Communication Style
{1–2 lines: how you communicate, what patterns you use}

## Tone & Personality
- {2–3 bullets: personality traits and behavioral tendencies}

## Constraints
- {1–2 bullets: what you never do, anti-patterns to avoid}
```

## Examples

**Shipping-focused persona:**
```markdown
# Pragmatic Builder

## Communication Style
Lead with the simplest working solution. State trade-offs concisely. Bias toward shipping.

## Tone & Personality
- Practical and results-oriented; favors working software over perfect abstractions
- Explicit about trade-offs: what you gain, what you give up, and why it is acceptable
- Comfortable with "good enough" when the alternative is delayed delivery

## Constraints
- Do not over-engineer or introduce abstractions without immediate need
- Never dismiss quality concerns; acknowledge them and prioritize deliberately
```

**Adversarial persona** (note the purpose comment — required for adversarial personas):
```markdown
# Relentless Challenger
<!-- Purpose: Forces deeper thinking by refusing surface-level answers. -->

## Communication Style
Ask pointed follow-ups on every response. Use "Why?", "What if that fails?", and "Prove it." Never accept the first answer.

## Tone & Personality
- Persistent and unrelenting but respectful; pressure is on ideas, not people
- Treat every claim as a hypothesis to test, not a fact to accept
- Escalate specificity with each challenge round

## Constraints
- Stop pushing when the answer is substantiated with concrete evidence
```

## Writing Rules

1. **No filler.** Every line carries information. Cut "You should", "It is important to", "Make sure to."
2. **Imperative voice.** "Lead with the simplest solution" not "You should lead with the simplest solution."
3. **Be behavioral, not theoretical.** "Ask 'why?' on every claim" not "Be inquisitive." Describe observable actions.
4. **Stay role-agnostic.** A persona must work with ANY role. If it only makes sense paired with one role (e.g., "review pull requests thoroughly" is a responsibility, not a personality), it belongs in a role file instead. Test: would this persona make sense on a teacher? A designer? A security tester? If not, it's too narrow.
5. **Constraints are anti-patterns.** Don't repeat what Tone already says. Constraints describe what the persona avoids — the failure modes that would break character.
6. **Adversarial personas get a purpose comment.** Add `<!-- Purpose: ... -->` after the title explaining the constructive intent.

## File Naming & Location

- Lowercase kebab-case, adjective-noun: `pragmatic-builder.md`, `wise-counselor.md`, `cold-analyst.md`
- Name should be self-descriptive without reading the file
- Path: `personas/{category}/{persona-name}.md`
- Categories: `engineering`, `product`, `design`, `learning`, `advisory`, `adversarial`
- New category only if 3+ files would belong there. Otherwise, use closest match.

## Workflow

1. **Clarify behavior** — what communication style, what personality traits, what tone?
2. **Check it's not a role** — if the user describes expertise or job function, that's a role, not a persona. Guide them to role-creator.
3. **Pick category** — which directory?
4. **Draft** — fill the 3-section template within token budget
5. **Self-check** — within budget? Imperative voice? Behavioral (not theoretical)? Role-agnostic? Constraints are anti-patterns?
6. **Save** — write to `personas/{category}/{persona-name}.md`

---
name: prompt-creator
description: >
  Generate role and persona markdown files for an AI prompt library. Use this skill whenever someone asks to
  create a new role, add a persona, generate a prompt file, expand the library, or requests something like
  "I need a role for X" or "create a persona that acts like Y." Also triggers when adding new categories,
  building agent configurations, or composing role+persona combos. If the request involves creating short,
  reusable prompt snippets for AI agents, this is the skill to use.
---

# Prompt Creator

Create role and persona markdown files that are short, composable, and immediately usable as system prompt snippets for AI agents.

Roles define **what** an agent does. Personas define **how** it behaves. They're designed to be mixed — any role works with any persona.

## Deciding: Role or Persona?

Before writing, determine which type the user needs:

- **Role** → job function, expertise, domain scope, deliverables. Answers "what does this agent know and do?"
- **Persona** → communication style, tone, personality. Answers "how does this agent talk and behave?"

If the user describes both (e.g., "a strict security reviewer"), split it: the role is `security-reviewer`, the persona is something like `strict-enforcer`. Composability matters more than convenience — two small files beat one bloated one.

## Token Budgets

These limits exist because LLM role adherence plateaus after ~200 tokens, and persona instructions over-index on style past ~150 tokens, hurting task performance. Staying within budget also preserves context for the actual task.

| Type | Lines | Tokens | Why |
|------|-------|--------|-----|
| Role | 8–15 | 100–250 | Anchors behavior without diluting task context |
| Persona | 5–10 | 60–150 | Modifies behavior effectively even when brief |
| Combined | ~20–25 | ~300 | Sweet spot for composable injection |

If you find yourself exceeding these limits, you're probably bundling two concepts. Split into separate files.

## Role Template

Every role file uses this exact structure. Fill each section — don't skip any, don't add extras.

```markdown
# {Role Name}

## Identity
{1–2 lines: who you are, seniority, domain}

## Core Expertise
- {2–3 bullets: specific technical/domain skills — name tools, patterns, methods}

## Responsibilities
- {2–4 bullets: what you do, what you deliver, what you review}

## Boundaries
- {1–2 bullets: what you defer to others, what's out of scope}
```

**Example 1 — Engineering role:**
```markdown
# Senior Backend Engineer

## Identity
You are a senior backend engineer with deep expertise in server-side systems, APIs, and data processing pipelines.

## Core Expertise
- Designing and implementing scalable services, RESTful/GraphQL APIs, and async workflows
- Performance optimization, caching strategies, and database query tuning
- Distributed systems patterns: circuit breakers, retries, eventual consistency

## Responsibilities
- Architect backend services for reliability and maintainability
- Write production-grade code with comprehensive error handling and logging
- Conduct code reviews focused on correctness, security, and performance
- Define API contracts and integration patterns

## Boundaries
- Defer frontend/UI decisions to frontend specialists
- Escalate infrastructure provisioning and networking to DevOps/platform teams
```

**Example 2 — Adversarial role** (note the constructive-purpose comment):
```markdown
# Devil's Advocate
<!-- Purpose: Strengthens proposals by surfacing overlooked counterarguments before they become real problems. -->

## Identity
You are a devil's advocate who systematically argues the opposing position on any proposal or decision.

## Core Expertise
- Identifying unstated assumptions and logical fallacies
- Constructing counterarguments from alternative stakeholder perspectives
- Stress-testing consensus for groupthink and confirmation bias

## Responsibilities
- Challenge every proposal with the strongest opposing case
- Surface risks, trade-offs, and second-order consequences the team missed
- Force proponents to provide evidence-backed justifications

## Boundaries
- Argue positions, not people. Yield when evidence conclusively addresses the objection.
```

## Persona Template

Every persona file uses this exact structure:

```markdown
# {Persona Name}

## Communication Style
{1–2 lines: how you communicate, patterns you use}

## Tone & Personality
- {2–3 bullets: personality traits and behavioral tendencies}

## Constraints
- {1–2 bullets: what you never do, anti-patterns to avoid}
```

**Example:**
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

## Writing Rules

These make the difference between a prompt that sticks and one the model ignores after two turns:

1. **No filler.** Every line must carry information. Cut "You should", "It is important to", "Make sure to."
2. **Imperative voice.** "Design systems for scale" not "You should design systems for scale."
3. **Be specific.** "Circuit breakers, retries, eventual consistency" not "distributed systems patterns." Name the tools, methods, and frameworks.
4. **Boundaries prevent overreach.** Without them, a backend role will happily redesign your CSS.
5. **Personas must be role-agnostic.** Write personas that work with ANY role. If it only makes sense paired with one role, the persona is too narrow.
6. **Adversarial files get a purpose comment.** An HTML comment at the top explaining the constructive intent: `<!-- Purpose: ... -->`. This signals that the adversarial behavior is deliberate, not malicious.

## File Naming

- Lowercase kebab-case: `senior-backend-engineer.md`, `patient-explainer.md`
- Self-descriptive without reading the file
- Roles: noun-based (`qa-engineer`, `tech-mentor`, `product-manager`)
- Personas: adjective-noun (`pragmatic-builder`, `wise-counselor`, `cold-analyst`)

## Directory Structure

```
roles/{category}/{role-name}.md
personas/{category}/{persona-name}.md
```

Categories: `engineering`, `product`, `design`, `learning`, `advisory`, `adversarial`

If a role doesn't fit existing categories, propose a new one — but only if at least 3 files would belong there. Otherwise, pick the closest match.

## Workflow

When asked to create a new role or persona:

1. **Clarify type** — is this a role, persona, or both?
2. **Pick category** — which directory does it belong in?
3. **Draft** — write the file following the template and budget
4. **Self-check** — verify: within token budget? Imperative voice? Specific skills named? Boundaries defined? Persona is role-agnostic?
5. **Save** — write to the correct path with kebab-case filename

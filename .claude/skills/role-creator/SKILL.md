---
name: role-creator
description: >
  Create role markdown files for an AI prompt library. A role defines WHAT an agent does — its expertise,
  responsibilities, and scope. Use this skill whenever someone asks to create a new role, add an agent role,
  needs "a role for X", wants to define what an agent specializes in, or is building agent configurations
  that need job-function definitions. Triggers on: "create a role", "I need a role for", "add a new role",
  "define agent expertise", or any request for a short reusable prompt that describes an agent's job function.
  Do NOT use this for persona/personality/tone requests — those go to persona-creator.
---

# Role Creator

Create role markdown files — short, composable system prompt snippets that define **what** an AI agent does.

A role anchors an agent's expertise, domain knowledge, and scope. It answers: "What does this agent know and do?" Roles are designed to pair with any persona (which defines *how* the agent behaves), so keep them personality-neutral.

## Token Budget

Role adherence in LLMs plateaus after ~200 tokens. Beyond that, extra instructions dilute rather than strengthen behavior. Staying compact also preserves context window for the actual task.

| Metric | Target | Danger zone |
|--------|--------|-------------|
| Lines | 8–15 | <5 drifts, >25 wastes context |
| Tokens | 100–250 | <60 too generic, >400 diminishing returns |

If you're exceeding the budget, you're probably blending two roles. Split them.

## Template

Use this exact structure. Four sections, no more, no less.

```markdown
# {Role Name}

## Identity
{1–2 lines: who you are, seniority level, domain}

## Core Expertise
- {2–3 bullets: specific skills — name tools, patterns, methods, not vague categories}

## Responsibilities
- {2–4 bullets: what you do, deliver, review}

## Boundaries
- {1–2 bullets: what you defer to others, what's out of scope}
```

## Examples

**Engineering role:**
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

**Adversarial role** (note the purpose comment — required for adversarial roles):
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

## Writing Rules

1. **No filler.** Every line carries information. Cut "You should", "It is important to", "Make sure to."
2. **Imperative voice.** "Design systems for scale" not "You should design systems for scale."
3. **Be specific.** "Circuit breakers, retries, eventual consistency" not "distributed systems patterns." Name the actual tools, methods, and frameworks.
4. **Boundaries prevent overreach.** Without them, a backend role will happily redesign your CSS. Every role needs explicit scope limits.
5. **Stay personality-neutral.** Roles define expertise, not tone. Don't include communication style — that's what personas are for. A role should work equally well paired with a "patient explainer" or a "blunt critic" persona.
6. **Adversarial roles get a purpose comment.** Add `<!-- Purpose: ... -->` after the title explaining the constructive intent. This signals the adversarial behavior is deliberate, not hostile.

## File Naming & Location

- Lowercase kebab-case, noun-based: `qa-engineer.md`, `tech-mentor.md`, `product-manager.md`
- Name should be self-descriptive without reading the file
- Path: `roles/{category}/{role-name}.md`
- Categories: `engineering`, `product`, `design`, `learning`, `advisory`, `adversarial`
- New category only if 3+ files would belong there. Otherwise, use closest match.

## Workflow

1. **Clarify scope** — what domain, what seniority, what does the agent need to know?
2. **Pick category** — which directory?
3. **Draft** — fill the 4-section template within token budget
4. **Self-check** — within budget? Imperative voice? Specific skills named? Boundaries defined? Personality-neutral?
5. **Save** — write to `roles/{category}/{role-name}.md`

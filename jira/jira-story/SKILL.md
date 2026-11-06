---
name: "jira-story"
description: "Use when writing a new Jira user story or enabler, or when reviewing and improving an existing story against the team's Definition of Ready (DoR), including a possible solution grounded in the repository."
---

# Jira story: create & review

This skill has two modes, both based on the team's Definition of Ready (below).

- **Create mode** — the user describes something they want built ("make a story for…", "write a ticket for…"). Produce a story in the template below.
- **Review mode** — the user pastes or links an existing story ("review this story", "is this ready?", "improve this ticket"). Check it against the DoR, report gaps, and give an improved version.

If it's unclear which mode is meant, infer from whether an existing story text is provided.

## Definition of Ready (source of truth)

The DoR is a quality check before an item is picked up in a sprint. A story is ready to be planned and worked on when:

**INVEST**
- Independent — can be built and delivered without waiting on other stories
- Negotiable — describes the need, leaves room for discussion on the solution
- Valuable — clear value for a user, the business, or (for enablers) the platform
- Estimable — the team has enough information to estimate it
- Small — fits comfortably in one sprint
- Testable — there is an objective way to verify it's done

**General**
- Nothing blocks implementation, testing, or deployment to production
- No intermediate approval is needed during implementation
- Business request: describes the business desire or goal in functional terms, **without technical details**
- Enabler: describes its purpose, technical requirements, and expected end result
- It is specified, in global terms, how to test the end result, and it has been checked that it *can* be tested

**Specified by the requestor**
- Title
- Description (WHAT)
- Context/Background (WHY — for whom, what is the added value)
- Functional acceptance criteria (when is it good enough to call done)

**Specified by someone in the team before refinement**
- Selected solution
- Scope
- Steps to deliver (HOW)
- Testing method (steps to verify the new functionality works as expected)
- Dependencies — if any remain open, the story is **not ready for sprint**
- Technical acceptance criteria

**During refinement (whole team)**
- Story points are assigned. A complex solution may be worked out in a separate pre-refinement; an obvious solution may be added before refinement. Every story still needs final commitment and pointing by the whole team.

**Definition — enabler:** infrastructure, a framework, or other technical work that facilitates creating and using business functionality.

## Repository context and possible solution

In both create and review mode, inspect the repository before drafting the team section. Read applicable repository instructions and use the README to locate the relevant area, then inspect the implementation, configuration, tests, and deployment files needed to understand the requested change. Follow the code when documentation and implementation differ, and state any material discrepancy.

For this repository, start with the relevant area rather than scanning everything: `aise-edp/` for Python ingestion, `aise-edp-jvm/` for JVM components, `Synapse/` for orchestration and artifacts, `databases/` and `meta-sqldb/` for database and metadata work, `Deployment_pipeline/` for CI/CD, and `powerbi/` for reporting. Treat these as navigation hints; verify actual paths and behavior for each ticket.

Include a **Possible solution (proposal)** in the team section:

- Describe a concrete approach that meets the requested outcome and fits the existing architecture. Prefer established patterns and reusable components found in the repo.
- Explain why the approach fits, cite the relevant repository-relative file paths (and symbols or line numbers where useful), and identify the components likely to change. Separate observed current behavior from proposed changes.
- Propose scope, delivery steps, a testing method, and technical acceptance criteria consistent with the approach. Identify existing tests or commands where available; label new tests as proposed. Do not claim that tests were run or that production testing is feasible without evidence.
- State assumptions, material tradeoffs, and dependencies that still need verification. Distinguish confirmed dependencies from possible ones; do not equate an absence of evidence with no dependencies.
- If repo access or relevant evidence is missing, explain the limitation and give only a conditional approach with explicit assumptions. Use `[TO DO: …]` for information that cannot be established. Never invent file paths, capabilities, or business facts.

Keep the proposal separate from **Selected solution**. Populate the selected solution only when the user or existing ticket identifies an agreed approach; otherwise mark it `[TO DO: Team to select a solution; see proposal below]`. Preserve an existing agreed solution unless the user asks to change it; explain any evidence-backed concern and present alternatives as proposals. A proposal alone does not satisfy the DoR requirement for a selected solution or close open dependencies.

Keep technical solution details in the team section for business stories. Drafting a solution does not authorize implementing it or changing repository files.

## Story template

Use this structure (Jira-friendly markdown). Write in the language the user writes in, unless they ask otherwise.

```
**Title:** <short, action-oriented, specific>
**Type:** Business story | Enabler

## Description (WHAT)
Business story: As a <role>, I want <capability>, so that <benefit>.
Enabler: Purpose, technical requirements, expected end result.

## Context / Background (WHY)
- For whom:
- Why now / problem today:
- Added value:

## Functional acceptance criteria
- [ ] ...

---
## Team section (complete before refinement)
**Selected solution:** <agreed approach, or [TO DO: Team to select a solution]>
**Possible solution (proposal):**
- Approach and rationale:
- Repository evidence / affected components:
- Assumptions and tradeoffs:
**Scope:** In scope: … / Out of scope: …
**Steps (HOW):**
1. ...
**Testing method:**
1. ...
**Dependencies:** None | <list — story is not sprint-ready while open>
**Technical acceptance criteria:**
- [ ] ...

**Story points:** _assigned by the team during refinement_
```

## Create mode

1. Determine the type: business story (functional need) or enabler (technical work). Ask if genuinely unclear.
2. If the WHY is missing (who benefits, what value), ask for it in one short question — it's the part most often skipped and it can't be invented. Otherwise draft straight away.
3. Fill the requestor section from what the user gave. For a business story keep the description and functional acceptance criteria free of technical details; put any technical ideas the user mentioned in the team section instead.
4. Fill the team section from the user's input and inspected repository evidence. Include a possible solution and proposed scope, delivery steps, testing method, and technical acceptance criteria using the guidance above. Label proposals and assumptions explicitly; do not present them as agreed decisions. Mark unresolved facts as `[TO DO: …]` with a short hint of what is needed. Never invent dependencies or estimates.
5. Never fill in story points.
6. Write acceptance criteria that are objectively checkable: concrete inputs, outputs, values, or observable behaviour. Avoid vague words (fast, easy, user-friendly, correct, robust) unless quantified.
7. If the story looks too big for one sprint or bundles separate goals, say so and propose a split into smaller stories.
8. End with a one-line status: what's still open before it's ready for refinement.

## Review mode

Output in this order:

1. **Verdict** — one of:
   - *Ready for sprint* — all DoR items met, including story points and no open dependencies
   - *Ready for refinement* — requestor and team sections complete, only pointing left
   - *Not ready* — with the one-sentence main reason
2. **DoR check** — a table with columns `DoR item | Status | Note`, using ✅ met, ⚠️ partial/weak, ❌ missing. Cover INVEST, the general items, the requestor items, and the team items. Keep notes short and specific (quote the weak phrase where useful).
3. **Top issues** — max 5, most important first, each with a concrete fix.
4. **Improved version** — the full story rewritten in the template. Keep the author's intent and facts; improve wording, structure and testability. Add a repository-grounded possible solution and proposed team-section details using the guidance above. Distinguish repo findings, proposals, and agreed decisions. Mark unresolved facts as `[TO DO: …]` rather than inventing them. The verdict and DoR check assess the supplied story using verified evidence; improvements and new proposals do not retroactively make the original story ready.

Things to watch for specifically:
- Technical details in the description of a business story (move them to the team section)
- Missing or vague WHY / added value
- Acceptance criteria that can't be objectively tested, or that restate the description
- Hidden dependencies (other teams, source systems, access rights, data availability, approvals) — an open dependency means not sprint-ready
- Need for intermediate approval during implementation
- Size: multiple goals, many systems touched, or "and" in the title often signal a split
- Enablers without a clear expected end result or link to the business functionality they enable

## Creating or updating in Jira

If a Jira connector is available, offer to create or update the ticket after the user has approved the text. Show the exact content and target project/issue first and wait for a clear yes before writing to Jira.
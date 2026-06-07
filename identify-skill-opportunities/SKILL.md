---
name: identify-skill-opportunities
description: Review the current agent session and project context to identify high-value opportunities to create, update, install, or skip reusable agent skills. Use when wrapping up a substantive project session, when the user asks whether repeated work should become a skill, when an agent notices recurring project-specific workflow, or when deciding whether a workflow belongs in a skill, script, automation, or documentation.
---

# Identify Skill Opportunities

Use this skill to turn real work into reusable agent capabilities. Ground recommendations in evidence from the current session and project artifacts, not generic speculation.

## Portability

Use host-neutral language and adapt to the current agent platform. A "skill" means any reusable instruction bundle, playbook, memory, tool profile, or workflow extension supported by the host. An "automation" means any scheduled, event-driven, reminder, monitor, or background workflow mechanism supported by the host. Project documentation can be any durable repo or workspace artifact that future humans and agents can read.

## Review Workflow

1. Inspect the current session for repeated steps, user corrections, project-specific facts, fragile command sequences, debugging patterns, and decisions that would have been easier with prior knowledge.
2. Inspect project context when useful and available: `README`, host-specific agent config directories, existing skills or playbooks, scripts, test commands, package files, CI config, runbooks, docs, and recent git history.
3. Check for reuse before creation: installed skills, project docs, existing scripts, and public skill registries when registry access is available.
4. Prefer installing or updating an existing skill when it covers most of the need. Create a new skill only for a coherent reusable workflow with local context or judgment that existing skills do not capture.
5. Classify each candidate as `skill`, `skill update`, `external skill`, `script`, `automation`, `project docs`, or `skip`.
6. Recommend only high-confidence candidates. If evidence is weak, say there are no strong skill opportunities.
7. Ask before creating or modifying any skill, script, automation, or documentation unless the user explicitly requested implementation.

## Classification Rules

Choose `skill` when the reusable value is judgment, project-specific procedure, domain knowledge, tool sequencing, validation strategy, or a recurring multi-step workflow that an agent may otherwise rediscover.

Choose `skill update` when an installed skill already covers the domain and the session revealed a concrete correction, gotcha, validation step, or project-specific pattern that belongs there.

Choose `external skill` when a reputable existing skill appears to solve the need without substantial local customization.

Choose `script` when the task is deterministic and repeatable with little judgment: formatting, file transforms, command wrappers, report generation, static checks, or structured extraction.

Choose `automation` when the task should happen on a schedule, after an event, or as a reminder or monitor. A skill may define how to decide or respond, but the scheduled behavior belongs in an automation.

Choose `project docs` when humans and agents both need stable project facts: setup steps, architecture notes, test commands, release process, service ownership, or operational runbooks.

Choose `skip` for one-off work, simple commands, obvious general engineering behavior, low-frequency tasks, or patterns already captured well enough elsewhere.

## Quality Bar

Recommend a candidate only when at least two signals are present:

- The user repeated or described the same workflow more than once.
- The agent had to rediscover project-specific facts or conventions.
- The user corrected the agent with information likely to recur.
- The task included fragile ordering, hidden prerequisites, or validation gates.
- The same command or file pattern appeared across multiple attempts.
- The workflow has meaningful cost when done wrong.
- The candidate can be described as one coherent capability.

Do not recommend a skill just because a task is common. Common tasks usually need a skill only when this project or user has non-obvious preferences, constraints, or failure modes.

## Reuse Before Creation

Before recommending a new skill, check whether the need is already solved:

- Inspect installed skills and project-level playbooks.
- If registry or internet access is available, search popular skill sources using task-specific keywords.
- Prefer reputable sources, active repositories, clear descriptions, and visible usage signals when comparing external skills.
- Verify the candidate skill's scope, trigger language, and safety posture before recommending it.
- Recommend a new local skill when the external option lacks project-specific gotchas, user preferences, validation steps, or workflow judgment.

Do not recommend third-party skill installation based on popularity alone. Popularity is a discovery signal, not a quality guarantee.

## Candidate Fitness Checks

For any `skill`, `skill update`, or `external skill` recommendation, include a quick fitness check:

- Trigger fit: write one phrase that should trigger the skill and one phrase that should not.
- Scope fit: name the coherent unit of work and what is explicitly out of scope.
- Structure fit: choose the likely shape: checklist, gotchas, rule set, references, scripts, or evals.
- Validation fit: propose 2-3 realistic eval prompts or real tasks to test whether the skill improves outcomes.

If the candidate cannot pass these checks, downgrade it to `project docs`, `script`, `automation`, or `skip`.

## Git And Repo Hygiene

Treat recurring git sync, branch cleanup, or upstream checks as follows:

- Recommend `script` for deterministic command sequences such as checking branch state or fetching upstream.
- Recommend `automation` for periodic checks such as "every 4 hours, remind me or report if my branch is stale."
- Recommend `skill` only when the workflow requires judgment: dirty worktree handling, rebase versus merge policy, recurring conflict resolution, project-specific validation, or risk reporting.

## Recommendation Format

Keep the output brief. Lead with the highest-value item.

```markdown
Skill opportunity review:

1. [create skill | update skill | external skill | script | automation | project docs | skip]: <candidate-name>
   Evidence: <specific session/project evidence>
   Reuse check: <existing skill/script/doc checked, or why not checked>
   Why this type: <short rationale>
   Fitness: <trigger/scope/validation summary>
   Suggested next step: <ask/create/install/defer/skip>
```

If there is no strong candidate, say:

```markdown
Skill opportunity review: no strong candidates from this session.
Reason: <one sentence>
```

## Creation Guidance

When the next step is to create or update a skill, follow the host platform's skill creation process or dedicated skill-authoring helper. Keep the new skill concise, grounded in real artifacts, and calibrated to the task. Prefer a small core instruction file with a checklist, gotchas, and output template before adding bundled scripts or references.

When referencing external skill-writing best practices, preserve these principles: start from real expertise, refine through real execution, spend context sparingly, provide defaults instead of broad menus, favor procedures over declarations, and include validation loops for fragile workflows.

## Evaluation

When tuning this skill, use `evals/evals.json`. The evals emphasize false-positive control: generic repetition, weak user hints, popularity-only external skills, and deterministic commands should not become new local skills.

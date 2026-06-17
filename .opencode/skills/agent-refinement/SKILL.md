---
name: agent-refinement
description: Use ONLY when refining an existing agent definition from a provided file path, using its current content and any user-provided docs or related skills as context.
---

# Agent Refinement

Use this skill when the user provides a concrete path to an existing agent definition and wants to improve, clarify, harden, or review it.

Do not use this skill to create agents from scratch. If the user asks for a new agent without a base document, ask for an existing base agent file first.

## Operating Mode

- Treat the existing agent file as the starting point and primary context.
- Treat user-provided documentation, examples, and related skills as supporting context.
- Do not edit the agent file unless the user explicitly asks for file changes.
- Prefer proposing concrete text blocks and explaining why they help.
- Keep the conversation in the user's language.
- Write proposed agent or skill content in English.
- Ask concise clarification questions whenever intent, scope, sources, or permissions are unclear.

## Initial Intake

Before recommending changes:

1. Read the provided agent file.
2. Ask whether the user has additional context such as domain docs, usage notes, examples, target behavior, related skills, or constraints.
3. Read related skills when they are referenced by the agent prompt, listed in `permission.skill`, or provided by the user as context.
4. Identify whether the user wants a broad review or a focused pass on one area such as `description`, permissions, sections, associated skills, or final cleanup.

Do not block forever waiting for extra documentation. If the user has none or asks to proceed, continue from the existing agent file alone.

## Recommended Iteration Flow

Use these phases when useful.

1. `description` and activation scope.
2. Frontmatter and YAML validity.
3. Mode, model, temperature, and options.
4. Permissions and tool policy.
5. Related skills and external documentation.
6. Prompt structure and section ordering.
7. Redundancy, contradictions, and missing boundaries.
8. Final review.

## Frontmatter Review

Agent and skill files are Markdown files with YAML frontmatter.

Check for:

- Useful `description` that says what the agent does and when to use it.
- Description phrasing that helps routing, using concrete trigger words the user is likely to say.
- Appropriate `mode`: `primary`, `subagent`, or `all`.
- Appropriate `temperature` for the task type.
- Required or useful `model`, `variant`, `color`, `hidden`, `disable`, or `options` only when justified.
- No unknown or accidental frontmatter fields unless intentionally used as options by the host tool.

## Description Guidance

Good descriptions should:

- Be specific enough for routing.
- Include the main domain, artifact types, and work mode.
- Say when to use the agent, not just what it is.
- Mention important constraints when they affect routing, such as read-only, review-only, web research, or implementation.
- Avoid over-broad descriptions such as `Agent`, `Helper`, or `Researcher`.

When a skill or agent must stay quiet on adjacent tasks, suggest `Use ONLY when...` phrasing.

## Permission Review

Review permissions against the agent's real job. Prefer least privilege.

Common patterns:

- Read-only researcher: allow `read`, `glob`, `grep`, `list`, `question`; deny `edit`, `bash`, and usually `task`; allow `webfetch` only if online research is in scope.
- Code editor: allow `read`, `glob`, `grep`, `list`, `edit`; use `bash` selectively for tests/builds; keep destructive commands denied or ask-gated.
- Reviewer: allow read/search tools; deny `edit` unless the user wants fixes; allow `bash` only if running checks is part of the review.
- Web or docs researcher: allow `webfetch` or `websearch` as needed; require source qualification and uncertainty notes.
- Coordinator or delegator: allow `task` only when subagent delegation is part of the design.

Also check:

- Whether `permission.skill` allows every skill the prompt expects to use.
- Whether the prompt references skills that do not exist or are denied.
- Whether `external_directory` is needed for user-provided paths, and whether `ask` would be safer than `allow`.
- Whether broad `allow` rules are justified by the agent's scope.
- Whether permissions contradict prompt claims such as read-only, no shell, no web, or no delegation.

## Related Skills Review

When related skills are present:

- Read their `SKILL.md` files before recommending agent changes.
- Check that the agent's instructions align with each skill's trigger conditions and workflow.
- Avoid duplicating detailed skill internals inside the agent unless the rule must apply before the skill is loaded.
- Prefer naming real skills exactly as they appear in frontmatter.
- Flag conceptual workflow names that look like skills but are not actual skills.

## Prompt Structure

Suggest only the sections the agent actually needs. A useful generic structure is:

- `Role`
- `Scope`
- `Non-goals`
- `Context sources`
- `Operating principles`
- `Workflow`
- `Tool and permission policy`
- `Associated skills`
- `Answer style`
- `Response format`

Prefer clear section names over many small sections. Avoid forcing this template onto small agents.

## Content Review Checklist

Look for:

- Missing domain definition when the domain acronym or context may be ambiguous.
- Missing scope or non-goals.
- Missing source priority or evidence policy.
- Missing workflow for how the agent should proceed.
- Missing answer style or output format guidance.
- Duplicate rules stated in multiple sections.
- Contradictions between frontmatter, permissions, and prompt body.
- Overly rigid response formats for simple questions.
- Instructions that assume unavailable tools, skills, paths, or external services.
- Claims that rely on model knowledge instead of provided sources.

## Recommendation Style

When responding to the user:

- Start with the practical recommendation.
- Separate must-fix issues from optional refinements.
- Provide concrete replacement text when helpful.
- Explain briefly why each recommendation helps.
- Preserve the user's ownership of edits when they said they will apply changes themselves.
- Do not provide patches or diffs unless explicitly requested.
- Mention that config-time changes require restarting the host tool when applicable.

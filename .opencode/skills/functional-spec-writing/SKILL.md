---
name: functional-spec-writing
description: Write formal functional specifications in Spanish, focused on observable behavior, real usage, outputs, errors, and limits.
---

## Use this skill when
- You need to create or rewrite a functional specification for a product, feature set, or user-facing workflow.
- You need to turn observed behavior, user notes, or stale documentation into a coherent functional document.
- You need another model or future session to follow the same specification standard.
- You need to review whether an existing functional specification is complete, consistent, and user-oriented.

## Hard rules
- Write the specification itself in Spanish.
- Keep a formal tone.
- Focus on user-visible behavior, real usage, outputs, errors, and limits.
- Do not describe implementation internals unless the user explicitly asks for technical documentation instead.
- Do not invent capabilities, flows, future work, or edge cases that are not supported by evidence.
- Use examples that match the real end-user interaction model.
- Keep numbering and terminology consistent from start to finish.
- If information is incomplete, degrade cautiously instead of filling gaps with assumptions.

## Minimum required inputs
- Product name.
- Core functional purpose.
- Real usage mode.
- At least one confirmed visible capability or user flow.
- At least one confirmed input, output, or user-visible result.

## Helpful but optional inputs
- Product type, such as CLI, web app, desktop app, API, service, or batch process.
- Target user.
- Accepted inputs.
- Generated outputs.
- Functional configuration.
- Prerequisites.
- Main errors and exceptional cases.
- Main limits and restrictions.
- Automatic behaviors.
- Behaviors that are present but not fully closed.

## Degradation rules
- If configuration details are missing, describe only the confirmed configuration or omit that detail.
- If errors are not confirmed, do not invent error cases.
- If limits are not confirmed, do not assume them.
- If outputs are not fully known, describe only the confirmed visible effect with cautious wording.
- If the target user is not clearly defined, write in neutral user-oriented language.
- If unfinished behaviors are not evidenced, do not add open points for them.
- If missing context prevents a reliable specification, ask for clarification before finalizing the document.

## Source priority
- Highest priority: real observed behavior.
- Next: reproducible usage evidence or verifiable tests.
- Next: current and reliable operational documentation.
- Lowest: comments, scattered notes, and legacy text.
- If behavior and old documentation conflict, trust behavior first.
- Do not copy existing documentation blindly.

## Inference and ambiguity
- Infer only what is clearly supported by evidence.
- Use conservative wording when the exact shape of a behavior is not fully certain.
- If something appears incomplete, reflect it carefully in behavior, limitations, or optional functional open points.
- If an ambiguity would materially change the specification, stop and ask for clarification before finalizing the document.

## Core model
- A functional specification explains what the product does for the user.
- It explains how the user interacts with it.
- It explains what the product needs in order to work.
- It explains what inputs it accepts, what outputs it produces, what the user can see, what can fail, and what limits exist.
- It must be understandable without code-level context.
- It does not explain how the product is implemented internally.

## Recommended document structure
- Use a numbered structure.
- Cover these topics unless one is truly not applicable to the product:
  - Introduction
  - Product purpose
  - Functional scope
  - Target user
  - Prerequisites for use
  - Functional configuration
  - Usage modes and available options
  - Usage examples
  - Accepted inputs
  - Special flow or content handling
  - Relevant parameter handling, when applicable
  - Automatic behaviors
  - Generated result
  - User-visible information
  - Errors and exceptional situations
  - Limitations
  - Functional open points, when useful
  - Usage scenarios
  - Operational, privacy, and cost considerations, when applicable
  - Short glossary
  - Final summary
- Preserve an existing section order if the user or repository already established one and it remains coherent.
- Do not force a rigid template when the document already has a valid structure.
- You may fuse sections when separating them does not improve clarity and functional traceability is preserved.
- Common valid fusions include:
  - Functional configuration + Relevant parameter handling
  - Special flow or content handling + Automatic behaviors
  - Usage modes and available options + Accepted inputs, for very small products

## Section guidance
- Introduction
  - State what the document is and what it describes.
  - Keep it functional, not technical.
- Product purpose
  - Explain the problem solved and the value delivered to the user.
- Functional scope
  - List the visible capabilities of the product.
  - Do not mix visible behavior with internal design.
- Target user
  - Describe who uses the product and what minimum context they need.
- Prerequisites for use
  - Describe what must be available before use.
  - Keep it operational and user-facing.
- Functional configuration
  - Describe only settings, values, or inputs that the user or operator supplies or configures.
  - State their meaning and functional effect.
- Usage modes and available options
  - Describe the visible ways the user can operate the product and the options they can choose.
- Usage examples
  - Show representative success and error cases.
- Accepted inputs
  - Describe valid input types, input conditions, and rejection cases.
- Special flow or content handling
  - Describe transformations, splitting, conversion, validation stages, or other special handling that matters to the user-visible flow.
- Relevant parameter handling
  - Describe precedence rules, defaults, persistence, or fallback logic when those matter functionally.
- Automatic behaviors
  - Describe what the product decides or performs without extra user input.
- Generated result
  - Describe what output exists, where it appears, and what it contains.
- User-visible information
  - Describe messages, confirmations, progress, warnings, or visible feedback.
- Errors and exceptional situations
  - Describe the main failure types and their functional consequence.
- Limitations
  - Describe real constraints, unsupported cases, or unfinished closures that affect the user.
- Functional open points
  - Use this optional section only when it adds real value.
  - Record only observed functional gaps that matter to close the specification.
  - Allowed content: pending functional decisions, open ambiguities, and validations needed to close the document.
  - Do not use it as a roadmap, task list, or technical backlog.
- Usage scenarios
  - Summarize end-to-end cases in plain language.
- Operational, privacy, and cost considerations
  - Cover external services, connectivity, credentials, sensitive data, and cost when relevant.
- Short glossary
  - Define only terms that help fix meaning for future readers.
- Final summary
  - Close with a faithful synthesis of the product behavior and limits.
- If two neighboring sections would repeat the same content, merge them instead of inflating the document.

## Example rules
- Include at least one example per main visible capability.
- Include the main error examples.
- Every example must contain exactly these three parts:
  - what it does
  - the real action, command, or user interaction
  - the expected result
- Prefer functional expected results over literal logs unless literal output is important to the user contract.
- If the product creates an artifact, file, record, or visible state change, mention it in the expected result.
- If the real final usage is a packaged command, executable, deployed URL, installed app, or public API call, use that real form in the example.
- If the exact output is not fully known, describe only the confirmed expected effect with cautious wording.
- Do not use build-only or developer-only invocations as final user examples unless the product is genuinely used that way.

## Adapt by product type
- CLI products
  - Use real commands, options, arguments, and invocation errors.
- Web products
  - Use visible user actions, forms, screens, and resulting states.
- Desktop products
  - Use menus, dialogs, actions, and visible results.
- APIs
  - Treat the API itself as the product.
  - Describe operations, consumer inputs, responses, and errors.
- Batch jobs and automation
  - Describe input source, execution trigger, generated outputs, logs or reports visible to operators, and operational failures.
- Always adapt the examples and wording to the actual user interaction model.

## Writing patterns
- Prefer functional verbs such as: allows, validates, generates, shows, rejects, processes, stores.
- Keep lists parallel.
- Make the relationship between user action and system response explicit.
- Favor stable wording that will age well.
- Keep sentences precise and direct.
- Use domain terms consistently.

## What to avoid
- Do not merge functional specification and technical design into one text.
- Do not rely on stale documentation when observed behavior says otherwise.
- Do not invent future work, speculative edge cases, or unsupported flows.
- Do not use examples that users will never run or follow in real life.
- Do not overload the document with literal console output or internal jargon when a functional description is enough.
- Do not let the summary promise behaviors that the rest of the document does not support.
- Do not let the limitations section contradict the scope section.

## Consistency checks
- Verify that functional scope matches the examples.
- Verify that limitations do not contradict visible capabilities.
- Verify that errors fit the described usage flow.
- Verify that output descriptions match the examples.
- Verify that fused sections still preserve clarity and functional traceability.
- Verify that any functional open points are based on evidence, not speculation.
- Verify that the final summary matches the body of the document.
- Verify that terminology stays stable from start to finish.

## Deliverable
- Produce a final document that is ready to read without extra explanation.
- Make it self-contained.
- Keep it formal, numbered, and user-oriented.
- Make it useful both for a human reader and for another model that needs to understand the product behavior.
- If the available information is incomplete, degrade the output cautiously instead of filling gaps with invented detail.
- Add functional open points only when they help close observed gaps in the specification.

## Validation checklist
- Is the document written in Spanish?
- Is the tone formal?
- Is the text centered on user-visible behavior?
- Does it avoid implementation internals?
- Is the chosen structure complete enough for the product type?
- Is numbering coherent?
- If sections were fused, is clarity still preserved?
- Is there at least one example per main capability?
- Are the main error cases represented?
- Does every example include action and expected result?
- If information was missing, was the output degraded without invention?
- If functional open points were added, are they based on evidence rather than roadmap thinking?
- Are scope, limitations, errors, and summary consistent with each other?

## Evolution rules
- Keep this skill generic across repositories and product types.
- Add new rules only when they prove reusable across multiple projects.
- Do not hard-code one repository's naming conventions, section titles, or preferred commands unless they are framed as examples.
- Favor durable principles over project-specific templates.
- Let the skill evolve by adding clearer rules and better checks, not by locking it to one exact document shape.
- Keep functional open points optional and evidence-driven.

## Optional extensions
- Add a coverage matrix when it helps map capabilities to sections.
- Add confidence notes only when uncertainty materially affects the document.
- Add a short quality rubric if you need to score a draft before delivery.
- Add product-specific supplements outside this core skill when a repository establishes extra rules.

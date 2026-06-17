---
name: ifs-docs-research
description: Use when searching or researching official IFS online documentation on docs.ifs.com, especially via indexed search because direct docs.ifs.com fetches are blocked.
---

# IFS Docs Research

Use this skill when the user asks to search, investigate, verify, or research anything in the official IFS online documentation at `docs.ifs.com`.

Do not use this skill for local codebase searches. It is specific to online documentation research.

## Version Selection

Infer the IFS documentation version from the project root folder name when the user does not specify a version.

The project folder name is expected to use this format:

```text
<major>.<release>.<patch>
```

Convert it to the docs version format:

```text
<major>r<release>
```

Examples:

```text
25.1.3   -> 25r1
24.2.16  -> 24r2
26.1.5   -> 26r1
```

Use the inferred value in URLs under:

```text
https://docs.ifs.com/techdocs/<version>/
```

If the folder name is ambiguous or does not match the expected dotted format, fall back to a broader `site:docs.ifs.com/techdocs` search without a version segment.

## Access Constraint

Direct `webfetch` calls to `https://docs.ifs.com/techdocs/<version>/...` return a browser verification page instead of the document content:

```text
Verifying your browser
Please wait while we verify your browser.
```

Do not try a direct fetch first. Search indexed results from the official `docs.ifs.com` domain instead.

## Recommended Workflow

1. Determine the documentation version from the user request or project root folder.

2. Search using DuckDuckGo HTML with `site:docs.ifs.com/techdocs/<version>`.

3. Use very specific queries containing API names, package names, page titles, and IFS concepts.

4. Treat search snippets as supporting evidence, not full documentation.

5. In the final answer, clearly state when the answer relies on official indexed snippets rather than direct page content.

## Useful Search Queries

Use `webfetch` against DuckDuckGo HTML URLs like these, replacing `<version>` with the inferred docs version:

```text
https://duckduckgo.com/html/?q=site%3Adocs.ifs.com%2Ftechdocs%2F<version>%20Post_Outbound_Message
https://duckduckgo.com/html/?q=site%3Adocs.ifs.com%2Ftechdocs%2F<version>%20Invoke_Outbound_Message
https://duckduckgo.com/html/?q=site%3Adocs.ifs.com%2Ftechdocs%2F<version>%20IFS%20Connect%20message%20routing
https://duckduckgo.com/html/?q=site%3Adocs.ifs.com%2Ftechdocs%2F<version>%20Routing%20Rules%20and%20Addresses
https://duckduckgo.com/html/?q=site%3Adocs.ifs.com%2Ftechdocs%2F<version>%20Bizapi%20Replacement
```

## Answering Guidance

When reporting back to the user:

- Mention that direct `docs.ifs.com` access is blocked by browser verification when relevant.
- Cite official indexed result titles/snippets when useful.
- Do not overstate snippets as a full documentation review.
- If the result is inconclusive, say that indexed snippets were insufficient.

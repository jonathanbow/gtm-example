# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A plain-markdown knowledge base documenting the GTM (go-to-market) automation
stack: what triggers what, what data moves between tools, and whether each
step is manual or automatic. There is no application code, build step, lint,
or test suite — this is documentation, and "correctness" means the docs
accurately reflect how the real automations behave.

## Structure

```
README.md              ← what this KB is and how to use it
apps.md                ← index of every app + which workflows touch it
workflows.md           ← index of every workflow (status, owner, apps)
_template.md           ← copy this to start a new workflow doc
workflows/
  <name>.md            ← one workflow per file
  <name>-diagram.html  ← optional visual diagram companion (see below)
```

## Editing workflow docs

- **One file per workflow**, in `workflows/`. Copy `_template.md` to start a
  new one — don't hand-roll the structure.
- Keep the step list **flat and sequential** — one step per numbered entry,
  in execution order. Don't nest steps.
- Every step documents: `Trigger` (Event / Rule / Manual), `Automation`
  (Automatic / Manual), `Direction` (`App A → App B`, or `internal` if it
  doesn't leave the app), `Data` (fields/entities that move), and `Owner`
  (person responsible if it breaks — `-` if fully automated with no human
  owner).
- **Whenever a workflow is added or retired, update both `apps.md` and
  `workflows.md`.** These two index files are the only place to answer "what
  breaks if X goes down" or "what's the status of Y" without opening every
  workflow file — they go stale fast if edits to `workflows/*.md` aren't
  mirrored there.
- Capture failure modes and manual gates in the `## Notes / failure modes`
  section of each workflow file — this is what makes the doc useful for
  incident response, not just onboarding.

## Building an HTML workflow diagram

Workflow files are plain markdown by design (readable in GitHub with zero
tooling), but a visual pipeline diagram is a useful companion for a workflow
someone needs to scan quickly rather than read. `workflows/lead-qualification-diagram.html`
is the reference example — reuse its conventions rather than inventing a new
visual language per workflow:

1. **Load the `artifact-design` skill before writing any markup.** It governs
   palette, typography, and layout choices — invoke it via the Skill tool
   first, don't skip straight to HTML.
2. **Treat this as a utilitarian, not editorial, artifact** — it's an
   internal ops document, not a landing page. Polish (real type hierarchy,
   a considered palette, both light/dark themes) still applies; a hero
   section or heavy motion doesn't.
3. **Follow the established visual pattern**: a vertical pipeline with
   numbered nodes down a connecting rail. Automatic steps use the accent
   color (solid rail); manual steps use a distinct color (dashed rail) and
   get visually bracketed as a "gate" since they're the exception, not the
   rule. Each step is a card showing: app tag, trigger/automation badges,
   direction, data chips, and owner. Known failure modes render as inline
   flagged callouts directly under the step they affect, plus a summary list
   at the end — pulled straight from the workflow's `## Notes / failure
   modes` section.
4. **Theme both light and dark** via CSS custom properties, overridden under
   `@media (prefers-color-scheme: dark)` and mirrored with
   `:root[data-theme="dark"]` / `:root[data-theme="light"]` so the viewer's
   theme toggle works in both directions.
5. **File naming**: `workflows/<workflow-name>-diagram.html`, alongside the
   source markdown file it visualizes.
6. **Write the file with no `<!DOCTYPE>`, `<html>`, `<head>`, or `<body>`
   wrapper** — just a `<title>`, `<style>`, and content markup — then publish
   it with the Artifact tool, which wraps it automatically. Re-publish to the
   same file path to update an existing diagram in place rather than minting
   a new URL.

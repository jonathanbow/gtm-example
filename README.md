# GTM Workflow Knowledge Base

Documents how apps in the GTM stack connect: what triggers what, what data
moves, and whether each step is manual or automatic.

## Structure

```
gtm-workflow-kb/
├── README.md          ← this file
├── apps.md            ← index of every app + which workflows touch it
├── workflows.md        ← index of every workflow (status, owner, apps)
├── _template.md        ← copy this to start a new workflow doc
└── workflows/
    └── lead-qualification.md   ← one real worked example
```

## How to use it

1. **One file per workflow**, in `workflows/`. Copy `_template.md` to start.
2. Update `apps.md` and `workflows.md` whenever you add or retire a workflow.
3. Keep the step table flat — one row per step, in order. Don't nest.

## Field definitions (step table columns)

| Field | Meaning |
|---|---|
| `#` | Order in the sequence |
| `App` | Tool where the step happens |
| `Action` | What actually happens, in plain language |
| `Trigger` | Event / Scheduled / Manual |
| `Automation` | Manual / Semi-auto / Automatic |
| `Direction` | `App A → App B` (source → destination). Use `internal` if it doesn't leave the app |
| `Data` | Fields or entities that move (comma-separated) |
| `Owner` | Person responsible if it breaks. `-` if fully automated with no human owner |

## Why this shape

- Plain markdown, no YAML — readable in GitHub/GitLab preview with zero setup.
- One workflow = one file, so diffs stay small and reviewable.
- The two index files (`apps.md`, `workflows.md`) are the only places you need
  to touch to answer "what breaks if X goes down" or "what's the status of Y."
- Because it's just tables and headers, it's easy to paste a whole file into
  Claude and ask questions like "what are all the manual steps here?"

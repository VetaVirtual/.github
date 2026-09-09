# .github

Organization-wide GitHub configuration for Veta Virtual. Files in this repo act as defaults for every repo in the `veta-virtual` org that doesn't define its own equivalent — so a change here propagates to every repo without copying or syncing.

To override for a specific repo, that repo can add a file with the same name to its own `.github/` folder.

## Contents

```
.github/
├── ISSUE_TEMPLATE/
│   ├── parent-issue.md
│   ├── sub-issue.md
│   └── bug-report.md
└── workflows/
    ├── zenhub-move.yml
    └── zenhub-move-pipeline.yml
```

## Issue templates

**parent-issue.md** — A shippable feature, scoped to ~4–6 weeks of engineering work and broken down into sub-issues. Each parent issue has a linked PRD in Notion and a corresponding row in the Engineering Roadmap tracker. Acceptance criteria are verifiable end-to-end in staging before close.

**sub-issue.md** — A single PR's worth of implementation work under a parent. One sub-issue per chunk; one PR per sub-issue. Linked to its parent via GitHub's native sub-issue UI.

**bug-report.md** — For bugs found internally (during development, QA, dogfooding, or via monitoring). Customer-reported issues flow through the **RT Dashboard Dev Requests** Notion database first and only graduate to a GitHub bug once engineering picks them up.

## Reusable workflows

Called from other repos with `uses: VetaVirtual/.github/.github/workflows/<file>@main` and `secrets: inherit` (they need the org secret `ZENHUB_TOKEN`).

**zenhub-move.yml** — Moves the issues named in a branch (`#N`, one or more) to one ZenHub pipeline. Inputs: `branch_name`, `repo_gh_id`, `workspace_id`, `pipeline_id` (`in_progress_pipeline_id` still works as the target for older callers). Used on branch creation (In Progress) and on merge into `staging` (On Staging).

**zenhub-move-pipeline.yml** — Moves every issue of one repo from one pipeline to another. Inputs: `repo_gh_id`, `from_pipeline_id`, `to_pipeline_id`. Used when `staging` is promoted into `main` (On Staging → Released).

## Updating

Edit the file here, open a PR, merge. Every repo in the org picks up the change within about a minute. No further action needed in other repos.

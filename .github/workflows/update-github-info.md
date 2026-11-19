---
name: update-github-info
description: Refresh the GitHub Info page with concise, useful updates from official GitHub sources, then request Mona's review.
intent: Keep the GitHub Info page accurate and practically useful by reflecting relevant GitHub Blog, Changelog, and Awesome Copilot workflow updates.
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
  copilot-requests: write
engine:
  id: copilot
  copilot-sdk: true
strict: true
network:
  allowed:
    - github.blog
    - github.com
    - awesome-copilot.github.com
tools:
  edit:
  web-fetch:
safe-outputs:
  create-pull-request:
    title-prefix: "[github-info] "
    reviewers: [mona]
    draft: false
    allowed-files:
      - site/content/github-info.md
    max-patch-files: 1
  noop:
---

# Update GitHub Info

Refresh `site/content/github-info.md` with a small, practical update based on official GitHub sources.

## Research

1. Read `notes/mona-notes.md` and `site/content/github-info.md`.
2. Use `web-fetch` to read:
   - https://github.blog/latest/
   - https://github.blog/changelog/
   - https://awesome-copilot.github.com/workflows/
3. Find the newest relevant items that help developers learn GitHub faster, especially about collaboration, Copilot, or Actions. Verify every new or changed fact against its source page.

## Update and review

- Preserve Mona's editorial guidance and the existing page's structure and tone.
- Keep summaries short and practical. Add or revise only a few useful points; do not turn the page into a news feed or rewrite unrelated content.
- Include a direct source link for every new or materially updated item, identifying whether it came from the GitHub Blog, Changelog, or Awesome Copilot workflows. Do not invent details or dates.
- Edit only `site/content/github-info.md`. Before proposing the change, review the diff and confirm it contains no unrelated edits.
- If none of the sources has a relevant update, a source cannot be verified, or the page needs no meaningful change, use `noop` with a brief reason.
- Otherwise, use the configured `create-pull-request` safe output to open a pull request requesting Mona's review. Do not write directly to the default branch or use any other write mechanism.

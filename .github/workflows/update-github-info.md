---
name: update-github-info
description: Keep the GitHub Info page current with concise, source-backed developer guidance.
intent: Help Mona keep the GitHub Info page useful and up to date for developers.
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
  pull-requests: read
  copilot-requests: write
strict: true
engine:
  id: copilot
  copilot-sdk: true
network:
  allowed:
    - github.blog
    - github.com
    - awesome-copilot.github.com
tools:
  edit: true
  web-fetch: {}
  github:
    mode: local
    toolsets: [pull_requests]
safe-outputs:
  create-pull-request:
    title-prefix: "[GitHub Info] "
    reviewers: [mona]
    draft: false
    allowed-files:
      - site/content/github-info.md
---

# Update GitHub Info

Read `notes/mona-notes.md` and `site/content/github-info.md` before proposing edits. Use the configured web-fetch tool to fetch:

- https://github.blog/latest/
- https://github.blog/changelog/
- https://awesome-copilot.github.com/workflows/

Update `site/content/github-info.md` only with recent, official information that adds practical value for developers. Keep summaries short, preserve the page's existing organization and voice, avoid duplicate or speculative claims, and link each new item to its specific GitHub Blog, Changelog, or Awesome Copilot workflows source.

Before editing, inspect open pull requests for an existing `[GitHub Info]` update. If one is already open, do not create a competing PR; call `noop` with a brief reason and let Mona review or close the existing PR. If neither source has a useful update or the page needs no change, call `noop` with a brief reason.

If either page cannot be fetched, or a required repository file cannot be read, stop and report the failure without editing or opening a PR. Otherwise edit only `site/content/github-info.md` and use the configured create-pull-request safe output to open a PR for Mona to review. Never write directly to the base branch or use direct GitHub write operations.

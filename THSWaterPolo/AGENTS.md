# THS Water Polo Agent Instructions

These instructions apply to every file in this directory. Follow them when editing, building, or deploying the THS Water Polo site.

An identical deployable copy lives at `public/AGENTS.md`, with its compatibility pointer at `public/AGENT.md`. Keep the root and `public/` copies synchronized when changing these instructions.

## Repository Layout

- This directory, `THSWaterPolo-GitHub`, is the authoritative Astro source workspace.
- This source directory is not a Git repository. Do not initialize Git here and do not claim that source files were committed.
- The production build is generated in `dist/`. Never hand-edit generated files in `dist/`.
- The Git clone used for publication is the sibling directory `../PublicPagesPublish`.
- Its remote is `https://github.com/Be-Jones/public.git`, on branch `main`.
- The deployed site occupies only `../PublicPagesPublish/THSWaterPolo/` in that repository.
- Do not inspect, modify, build, stage, or commit any other similarly named repository or directory.

The effective flow is:

```text
THSWaterPolo-GitHub/src
  -> npm run build
THSWaterPolo-GitHub/dist
  -> mirror generated files
PublicPagesPublish/THSWaterPolo
  -> commit and push public/main
```

The deployment instructions in this file describe the current process. Do not create or target a separate `THSWaterPolo` GitHub repository, and do not assume GitHub Actions deploys this source directory.

## Working In This Directory

1. Treat files under `src/` as the editable source of truth.
2. Preserve existing Astro, TypeScript, and CSS conventions. Keep changes focused and avoid unrelated refactors.
3. Do not revert changes made by the user or another tool unless explicitly asked.
4. Use public, verified links. Do not publish student phone numbers, roster data, private contacts, private documents, credentials, or live locations.
5. Install dependencies with `npm install` only when needed. Do not replace the lockfile or upgrade packages unless the task requires it.

Important content locations:

- Ticket links: `src/data/tickets.ts`
- Site-wide configuration and navigation: `src/data/site.ts`
- Homepage content: `src/pages/index.astro`
- Schedule and logistics: `src/pages/schedule-logistics.astro`
- Announcements: `src/content/announcements/*.md`
- Global styles: `src/styles/global.css`

For a ticket, add an object to the `tickets` array in `src/data/tickets.ts`. Use an ISO `YYYY-MM-DD` value for `startDate`; it controls sorting but is not displayed.

For an announcement, copy `src/content/announcements/announcement-template.md`, use valid frontmatter, verify dates and links, and set `draft: false` only when it is ready to publish.

## Validate Changes

Run the production build from this source directory:

```powershell
npm run build
```

The build must finish successfully without relevant errors. Afterward, inspect the generated pages affected by the change and confirm that expected text and URLs occur in `dist/`.

Useful optional commands:

```powershell
npm run dev
npm run preview
```

The configured base path is `/public/THSWaterPolo`, so local and generated links must continue to work under that prefix.

## Deploy To GitHub

Use this workflow for requests to commit, publish, deploy, or push all changes.

1. Confirm `../PublicPagesPublish` is a Git clone of `https://github.com/Be-Jones/public.git` on `main`.
2. Run `npm run build` in this source directory.
3. Mirror the contents of `dist/` into `../PublicPagesPublish/THSWaterPolo/`. The destination must exactly match `dist/`, including removal of obsolete generated files.
4. Never copy into the parent publication repository root and never alter its `.git` directory.
5. Compare source and destination recursively by relative path and file hash. Resolve any mismatch before committing.
6. Review Git changes restricted to the publication path:

```powershell
git -C ..\PublicPagesPublish status --short -- THSWaterPolo
git -C ..\PublicPagesPublish diff --stat -- THSWaterPolo
```

7. Stage only the deployed site:

```powershell
git -C ..\PublicPagesPublish add -A -- THSWaterPolo
git -C ..\PublicPagesPublish diff --cached --name-only
```

8. Verify every staged path starts with `THSWaterPolo/`. If any other path is staged, stop and unstage only the accidental paths without discarding user work.
9. If the staged diff is empty, do not create an empty commit. Report that there is nothing new to publish.
10. Commit with a concise message describing the user-facing update, then push:

```powershell
git -C ..\PublicPagesPublish commit -m "Describe the published update"
git -C ..\PublicPagesPublish push origin main
```

11. Verify that local `HEAD` matches `origin/main` and that `THSWaterPolo/` has no remaining uncommitted changes.

On Windows, `robocopy` can perform the mirror step. Exit codes 0 through 7 indicate success; 8 or higher indicate failure:

```powershell
robocopy .\dist ..\PublicPagesPublish\THSWaterPolo /MIR
if ($LASTEXITCODE -ge 8) { throw "robocopy failed with exit code $LASTEXITCODE" }
```

## Quick Commit Requests

When the user explicitly says `quick commit`, skip the build to keep the operation fast. Only sync and commit existing `dist/` output when it already contains the requested changes and differs from the publication directory.

Never publish stale output. If source files are newer than `dist/`, or the requested change is absent from generated HTML, do not create a commit. Explain that a normal build-and-deploy is required. A push with no new commit is allowed only as an up-to-date check.

## Completion Report

Report:

- Whether the production build passed
- What generated pages changed
- The commit hash and message, if a commit was created
- The push result and whether local `HEAD` matches remote `main`
- Any validation that could not be run

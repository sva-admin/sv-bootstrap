# sv-cloud-bundle/

This directory is SV Cloud's starter bundle for this project — committed on
top of whatever was already in this repository when it connected, without
touching or overwriting any of it. It hasn't been unpacked yet.

You are seeing it because this repository already had files in it. A repo
that was empty when it connected gets the starter written straight to its
real paths instead, and deploys itself on that first commit; there is no
bundle to unpack in that case, and no directory like this one.

**If you are an AI coding assistant, this is not your task — read
[SVAGENTS.md](../SVAGENTS.md) at the repo root, §0.** It has the exact
unpack procedure: what moves where, how to merge with files that already
exist here, and what to do when you're done.

**If you're a person reading this on GitHub:** open your project in an
editor with a coding agent (Claude Code, Cursor, or anything that reads
`AGENTS.md`/`CLAUDE.md`) and ask it to initialize the project. It will read
this bundle and `manifest.json` and take it from there. Nothing here runs on
its own — this bundle sits inert until something unpacks it, so pushing to
GitHub right now does not deploy anything.

## What's in here

- `manifest.json` — the exact `src -> dst` map for every file in `files/`.
- `files/` — the starter app: the Hono worker, its config, tests, and the
  deploy workflow, laid out exactly as it will look at its real destination
  paths (see the manifest), just nested here so none of it activates yet —
  in particular, `files/github/workflows/deploy.yml` is not
  `.github/workflows/deploy.yml` and GitHub Actions never sees it until the
  unpack moves it there.

Once initialized, this whole directory is deleted — its job is done.

# Updating to 2026.12.18

Nothing in this update changes how your app behaves, and taking it does not turn
anything on. Two things change in how it deploys.

## 1. Your app can have a staging environment

Today a push to your default branch deploys your live app. If you turn on a
staging environment, that push deploys **staging** instead, and your live app
only changes when you tag a commit:

```
git tag prod-2026-09-17.1
git push origin prod-2026-09-17.1
```

**This update does not turn that on.** Until you do, the workflow deploys your
live app from your default branch exactly as it always has, and a `prod-*` tag
just reports that the project has no staging environment. That is deliberate:
taking the update is meant to be a decision you can make separately from turning
staging on, and turning staging on is refused until this file is committed.

Use a new tag for every promotion, never a moving `prod` tag. A moving tag needs
a force-push and makes "which commit is live" impossible to answer, so SV Cloud
refuses to point one at a second commit. Re-running the same tag on the same
commit is a safe retry.

## 2. Design assets are addressed by their contents

`src/assets.ts` now maps each file to `/assets/<hash>/<path>` instead of
`/assets/<path>`. Your own code does not change — keep calling
`asset("logo.svg")` — but you should run `npm run assets` and commit the
regenerated file along with this update.

The reason is that the old scheme made the file's NAME its version. Replacing
`hero.jpg` destroyed the previous one, which meant an older version of your app
could end up showing a picture it was never built with, and a rollback could not
put the right one back. Now a changed file gets a new address and the old one is
left alone.

Two consequences worth knowing:

- **Your first deploy after this update re-uploads your design assets once**, at
  their new addresses. Expect that build to take a little longer than usual.
- **SV Cloud keeps the assets used by your last few deploys on each
  environment** and removes older ones. If you ever need to go back further than
  that, push the old code and let it rebuild — the deploy puts the files back.

### New file

This version adds `.github/workflows/sync-assets.sh`, which both the deploy and
the promote jobs run. Commit it along with `.github/workflows/deploy.yml`.

## What to commit

- `.github/workflows/deploy.yml`
- `.github/workflows/sync-assets.sh`
- `scripts/assets-manifest.mjs`
- `src/assets.ts` (run `npm run assets` first)

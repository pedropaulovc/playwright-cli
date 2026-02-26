# Playwright CLI Fork

This is a fork of the official `@playwright/cli` npm package (`microsoft/playwright-cli`) with a fix for the `program.parse()` issue and repointed to use `@pedropaulovc/playwright` fork packages.

## The Fix

Upstream's `playwright-cli.js` does `require('playwright/lib/cli/client/program')` expecting it to auto-execute. But the `program` module was refactored to export a `program` object without calling `parse()`. This fork adds the missing `program.parse(process.argv)` call.

## Branches

- **feat/fix-program-parse** - Based off upstream main, contains only the `program.parse()` fix. Clean enough for a PR to upstream.
- **fork/main** (default) - Based off feat/fix-program-parse, renames package to `@pedropaulovc/playwright-cli` and points dependencies at the fork.

## Keeping Branches in Sync

```bash
git fetch upstream
git checkout feat/fix-program-parse
git rebase upstream/main
git push origin feat/fix-program-parse --force-with-lease

git checkout fork/main
git rebase feat/fix-program-parse
git push origin fork/main --force-with-lease
```

**Important:** Always rebase, never merge.

## Published Package

- `@pedropaulovc/playwright-cli`

## Publishing

Trigger the `sync-upstream.yml` workflow in GitHub to publish:

```bash
gh workflow run sync-upstream.yml --ref fork/main -f force_publish=true -R pedropaulovc/playwright-cli
```

## Testing

After publishing, validate in the codjiflo project:

```bash
cd /home/pedro/src/codjiflo/F
# Ensure package.json has: "@pedropaulovc/playwright-cli": "0.1.1.1"
npm install
npx playwright-cli --version
```

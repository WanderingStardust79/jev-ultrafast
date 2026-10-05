# Customizations

Personal fork of [browser-use/jev-ultrafast](https://github.com/browser-use/jev-ultrafast), tracking upstream, not contributing back. My changes live on the `mattb/main` branch; `main` stays clean for upstream comparison.

Adopted 2026-10-04. Security vetted at upstream commit `1231850a0bf1a0c0341fe408ef1668dbbfdfac46`. Later upstream commits are NOT vetted.

## Changes

- 2026-10-04: `CUSTOMIZATIONS.md`, `.github/workflows/sync-upstream.yml`: added by repo-adopt. Sync is fetch only (see below).

## Syncing upstream

The weekly Action only reports new upstream commits and the files they touch. It never merges or pushes. To take an update, review the upstream diff first, then:

```bash
git fetch upstream
git diff 1231850a0bf1a0c0341fe408ef1668dbbfdfac46..upstream/main --stat
git checkout main && git merge --ff-only upstream/main && git push origin main
git checkout mattb/main && git merge main
```

## License reminder

Upstream license: **MIT**. Permissive, adapt freely. Attribution required if you redistribute.

## Vetting conditions

Verdict: **CONDITIONAL** (4 stage scan, 2026-10-04: no malicious code or prompt injection found).

- Runs inside your real Chrome profile with your logged in sessions and no site allowlist. Use a dedicated Chrome profile with no logins. Never point it at UCPM systems, Yardi, AppFolio, banking or email.
- Each step sends the goal, URL, page title, up to 6,000 characters of visible text and the control list to api.typesafe.ai. Blocked for UCPM data until TypeSafe is named in writing (ucpm-data-privacy).
- model.py silently falls back to https://api.deepseek.com/v1 if TEXT_MODEL_BASE_URL is unset. Set TEXT_MODEL_BASE_URL, TEXT_MODEL, TEXT_MODEL_REASONING and both API keys explicitly in .env. Keep .env out of git.
- The decision model is hosted by TypeSafe (TYPESAFE_API_KEY needed). Customizations reach only the executor and snapshot code.
- Disable browser-harness telemetry before first use: BH_TELEMETRY=0 or `uv run browser-harness telemetry disable`.
- Known open bug (upstream issues 191, 198): cached text can be typed into a different field with the same label. Do not use on forms where a wrong entry is costly.
- Before merging upstream, diff jev_ultrafast/, pyproject.toml and uv.lock. Watch for new dependencies, subprocess calls, new outbound URLs, and changes to model.py or browser.py. Re vet on every merge.
